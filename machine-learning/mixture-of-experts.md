---
title: "Mixture of Experts (MoE)"
aliases: ["mixture of experts", "MoE", "mélange d'experts", "sparse MoE"]
tags: [machine-learning, transformer, llm, architecture, sparsity]
created: 2026-06-16
source: conversation with Claude
related: ["[[transformer-architecture]]", "[[llm-inference-optimization]]", "[[how-neural-networks-train]]", "[[attention-mechanism]]", "[[distillation]]"]
---

# Mixture of Experts (MoE)

## TL;DR

> Le **Mixture of Experts** n'est pas une architecture rivale du Transformer : c'est une modification **interne** au Transformer. Il remplace le MLP dense de chaque bloc par **N petits MLP (les experts) + un routeur** qui n'en active qu'un sous-ensemble (top-k) par token. Résultat : on **découple le savoir total du modèle (paramètres totaux) de son coût de calcul par token (paramètres actifs)**.

## Key concepts

- **Expert** — un simple MLP (le bloc $W_{\text{up}}/W_{\text{gate}}/W_{\text{down}}$). Pas un Transformer complet : ni attention, ni embeddings, ni couches propres.
- **Routeur (gating network)** — petite couche linéaire qui score les experts et sélectionne les **top-k** pour chaque token.
- **Top-k routing** — on n'active que les $k$ experts les mieux scorés (typiquement $k=2$ sur 8, 64 ou 256 experts).
- **Paramètres totaux vs actifs** — tous les experts occupent la mémoire (totaux, énorme) ; seuls $k$ tournent par token (actifs, faible).
- **Calcul conditionnel / sparse** — chaque token ne traverse qu'une fraction du réseau, choisie dynamiquement.
- **Load-balancing loss** — perte auxiliaire qui force le routeur à répartir les tokens équitablement, sinon il s'effondre sur quelques experts.

## Deep dive

### Ce que MoE change (et ne change pas)

Un modèle est « un Transformer » par son architecture globale : la pile de blocs identiques, chacun contenant **attention + MLP**, encadrés de normalisations et de résiduels (voir [[transformer-architecture]]). MoE ne remplace **rien** de cela. Il ne modifie **qu'un seul composant** : le MLP dense de chaque bloc devient un ensemble d'experts-MLP + un routeur.

La formulation correcte est donc : Mixtral, DeepSeek, GPT-4 (etc.) sont des **Transformers qui utilisent des couches MoE**. « MoE » est un adjectif sur le FFN, pas une architecture concurrente. Ce n'est jamais « Transformer **ou** MoE » — c'est toujours les deux à la fois.

Deux éléments restent **denses et partagés par tous les tokens** :
- l'**attention** (tout token attend toujours via les mêmes $Q/K/V$) ;
- la plomberie résiduels/normalisation.

Seul le MLP est « expertisé ».

```
Bloc Transformer (variante MoE) :
  x
  → LayerNorm
  → Attention          ← INCHANGÉE, dense, partagée par tous les tokens
  → + résiduel
  → LayerNorm
  → Routeur sélectionne top-k experts ┐
       Expert 1 (un MLP)              │  ← seul le MLP est splitté
       Expert 2 (un MLP)              │
       ... Expert N (un MLP)          ┘
  → + résiduel
  x_out
```

Le découpage est **par couche** : un modèle à 32 couches a 32 ensembles d'experts indépendants et 32 routeurs indépendants. Le routage est **redécidé à chaque couche** — un token peut aller à l'expert 3 en couche 5, puis à l'expert 7 en couche 6.

### Pourquoi MoE attaque le MLP précisément

D'après [[transformer-architecture]], le MLP concentre ~60–70 % des paramètres et joue le rôle de **mémoire associative** (c'est là que vivent les « faits »). Or, dans un Transformer dense, **chaque token active la totalité de ces poids**, même quand l'écrasante majorité des « faits » stockés sont hors-sujet : un token sur la cuisine française allume les mêmes neurones qu'un token sur la syntaxe Python. MoE supprime ce gaspillage en n'activant que les experts pertinents.

### Le mécanisme du routeur

Le routeur est minuscule : $\text{logits} = W_{\text{router}} \cdot x$, puis softmax, puis on garde les $k$ meilleurs. La sortie de la couche MoE est la **somme pondérée** des sorties des experts choisis, pondérée par leurs scores softmax :

$$y = \sum_{i \in \text{top-}k} g_i(x)\, E_i(x), \qquad g(x) = \text{softmax}(W_{\text{router}}\, x)$$

où $E_i$ est l'expert $i$ et $g_i$ son poids de gating.

### Comment le routeur est entraîné

Pas d'étiquettes « ce token doit aller à l'expert 4 », pas d'entraînement séparé. Le routeur est appris **end-to-end par la même backprop** que le reste du modèle (voir [[how-neural-networks-train]]).

Le ressort : comme la sortie des experts choisis est **multipliée par les poids softmax du routeur**, le gradient remonte jusqu'à $W_{\text{router}}$. Si l'expert 2 a produit un résultat utile, la perte baisse, et le gradient pousse $W_{\text{router}}$ à donner un score plus élevé à l'expert 2 pour des tokens similaires. La **spécialisation est émergente, jamais assignée** — exactement la dynamique de divergence forcée décrite dans [[attention-mechanism]] (les « trois ouvriers identiques » qui se spécialisent par leur position dans la chaîne).

### L'effondrement et la perte d'équilibrage

Laissé seul, ce routeur s'effondre par auto-renforcement : un expert légèrement meilleur au départ reçoit plus de tokens → s'entraîne plus → devient encore meilleur → capte *tous* les tokens. Les autres meurent de faim (*rich get richer*).

Pour l'empêcher, on ajoute une **perte d'équilibrage auxiliaire** à la perte principale :

$$\mathcal{L} = \mathcal{L}_{\text{principale}} + \alpha \cdot \mathcal{L}_{\text{balance}}$$

avec $\alpha$ petit (≈ 0,01). $\mathcal{L}_{\text{balance}}$ mesure l'inégalité de répartition des tokens entre experts et la pénalise. Le routeur subit donc **deux pressions simultanées** : la perte principale le pousse vers un routage *utile* (envoyer le token là où il aide), la perte d'équilibrage vers un routage *équitable* (ne pas affamer d'experts). Le routeur final est le compromis.

Mécanismes annexes : **bruit** sur les logits du routeur (*noisy top-k gating*) pour favoriser l'exploration, et **capacité** plafonnée par expert (les tokens en surplus sont *dropped* ou passent tels quels via le résiduel).

### Le vrai coût : mémoire, pas calcul

MoE déplace le goulot d'étranglement exactement dans le régime décrit par [[llm-inference-optimization]] : l'inférence est **bornée par la mémoire, pas par le compute**.

- **On économise des FLOPs, pas de la mémoire.** Tous les experts doivent résider en VRAM même si seuls $k$ tournent. Un modèle « 13B actifs » garde l'empreinte mémoire d'un 47B.
- **Communication** : les experts sont souvent répartis sur plusieurs GPU (*expert parallelism*), donc router = transférer des tokens entre devices.
- Côté inférence, le routeur est **figé** : il fait juste top-k et exécute. D'où l'avantage (calcul par token faible) et la rançon (coût mémoire de tous les experts).

## Examples & analogies

- **Mixtral 8×7B** : ~47 milliards de paramètres totaux, mais seulement ~13 milliards activés par token (top-2 sur 8 experts).
- **L'hôpital et l'infirmière de tri.** Un modèle dense est un médecin généraliste unique qui examine chaque patient avec tout son savoir — exhaustif mais lent et gaspilleur. Un MoE est un hôpital avec une **infirmière de tri (le routeur)** qui envoie chaque patient aux 2 spécialistes les plus pertinents sur 64. L'hôpital sait collectivement bien plus (capacité totale énorme), mais chaque patient ne mobilise que deux spécialistes (calcul actif faible). Rançon : il faut quand même **payer les 64 spécialistes pour être présents** (mémoire), et l'infirmière a intérêt à bien trier.

## Open questions

- Comment MoE recompose-t-il les **scaling laws** ? (croissance de la capacité totale sans croissance proportionnelle du compte par token)
- **MoE fine** vs **gros experts** : DeepSeek-V3 et ses centaines de petits experts + experts « partagés » toujours actifs — quel arbitrage ?
- Interaction MoE × **quantization** du KV cache : où se situe alors le vrai goulot mémoire ?
- Peut-on **distiller** un gros MoE vers un modèle dense plus petit, et que perd-on ? (voir [[distillation]])

## Related notes

- [[transformer-architecture]]
- [[llm-inference-optimization]]
- [[how-neural-networks-train]]
- [[attention-mechanism]]
- [[distillation]]
