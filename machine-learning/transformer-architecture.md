---
title: "L'architecture Transformer"
aliases: ["transformer", "GPT architecture"]
tags: [machine-learning, transformer, llm, architecture]
created: 2026-05-26
source: conversation with Claude
related: ["[[neural-networks-overview]]", "[[how-neural-networks-train]]", "[[attention-mechanism]]", "[[embeddings-and-tokenization]]", "[[llm-inference-optimization]]"]
---
# L'architecture Transformer

## TL;DR

> Le Transformer (Vaswani et al., *Attention Is All You Need*, 2017) est l'architecture qui domine les LLM modernes. Il enchaîne des blocs identiques (attention + MLP, encadrés de normalisations et de connexions résiduelles) sur des séquences de tokens représentés en vecteurs. Sa force : il est parallélisable et capture les dépendances longues sans dégradation du signal.

## Key concepts

- **Couche de Transformer** — sous-bloc identique, répété 32 à 100+ fois. Contient une attention + un MLP, avec normalisations et résiduels autour.
- **Connexion résiduelle** — $x + f(x)$ au lieu de $f(x)$. Stabilise l'entraînement et permet au gradient de remonter facilement.
- **LayerNorm** — normalise les activations couche par couche pour éviter les explosions/extinctions.
- **MLP / FFN** — bloc Feed-Forward où vivent ~60–70 % des paramètres. Mémoire associative qui stocke les "faits" du modèle.

## Deep dive

### Pipeline complet d'un LLM

```
texte
 → tokenisation (BPE) → IDs
 → embedding → un vecteur par token
 → + position encoding (RoPE)
 → N couches de Transformer
 → un-embedding → distribution sur vocabulaire
 → softmax → probabilités
 → échantillonnage → token suivant
 → boucle (autorégressif)
```

Voir [[embeddings-and-tokenization]] pour l'entrée et la sortie, et [[attention-mechanism]] pour le cœur d'une couche.

### Anatomie d'une couche

Une couche n'est pas juste "matrice × vecteur" : c'est un sous-graphe avec normalisations et résiduels.

```
x_in
 ├─ (gardé pour résiduel)
 → LayerNorm
 → Attention (Q, K, V, O)
 → + x_in                    ← première résiduelle
 ├─ (gardé)
 → LayerNorm
 → MLP (W_up, W_gate, W_down)
 → + précédent               ← deuxième résiduelle
x_out
```

### Le MLP : mémoire associative

Différence clé avec l'attention :
- **Attention** mélange l'info **entre tokens**.
- **MLP** transforme chaque token **indépendamment**.

Forme classique (GPT-2, BERT) :

$$y = W_{\text{down}} \cdot \text{activation}(W_{\text{up}} \cdot x + b)$$

Dimension cachée typique : $4 \times d_{\text{model}}$. Activations : GELU, SiLU.

Forme moderne (Llama, SwiGLU) :

$$y = W_{\text{down}} \cdot \big(\, \text{SiLU}(W_{\text{gate}} \cdot x) \;\odot\; (W_{\text{up}} \cdot x) \,\big)$$

(où $\odot$ est le produit terme à terme, *element-wise*.)

La recherche récente suggère que les MLP fonctionnent comme une **mémoire associative géante** : chaque neurone caché détecte un motif particulier et active une réponse. C'est là que les "faits" vivent ("Paris est la capitale de la France"), pas dans l'attention.

### Pourquoi plusieurs couches

- **Sans non-linéarité entre, plusieurs couches valent une seule** : $W_2 \cdot (W_1 \cdot x) = (W_2 \cdot W_1) \cdot x = W' \cdot x$. Sans ReLU/GELU/SiLU intercalés, GPT-4 serait une régression linéaire.
- **Composition d'opérations** : 32 couches = 32 étapes successives de raffinement.
- **Hiérarchisation observée empiriquement** : couches basses → syntaxe, milieu → sémantique, hautes → tâche-spécifique.
- **Profondeur de calcul** = profondeur de raisonnement disponible.

### Pourquoi le Transformer a tout changé (2017)

- **Parallélisable** — tous les tokens calculent leurs Q/K/V en parallèle pendant l'entraînement (vs RNN séquentiel). Massif sur GPU.
- **Dépendances longues sans dégradation** — n'importe quel token peut regarder n'importe quel autre directement via [[attention-mechanism|l'attention]], peu importe la distance.
- **Scaling laws** — performance prévisible avec la taille. A justifié les investissements massifs.

### Répartition typique des paramètres

| Composant | Part des paramètres |
|---|---|
| MLP | ~60–70 % |
| Attention (Q, K, V, O) | ~25–35 % |
| Embeddings + reste | quelques % |

L'attention est l'innovation conceptuelle, mais c'est dans les MLP que vit la majorité du modèle.

## Examples & analogies

- **Attention = "à quoi penser", MLP = "ce que je sais à ce sujet".** Cette dichotomie aide à se souvenir des deux rôles.
- **Couches comme étapes de raffinement** : pour comprendre "Le chat que Marie a adopté hier dort", il faut d'abord reconnaître les mots, puis résoudre les liens grammaticaux ("que" → "chat"), puis désambiguïser, puis intégrer. Une seule couche ne le ferait pas.

## Open questions

- Quelles alternatives au Transformer émergent ? (Mamba, RWKV, state-space models)
- Peut-on dépasser la complexité quadratique de l'attention sans perdre en qualité ?
- Quelle est la limite naturelle des scaling laws — y a-t-il une fin ?

## Related notes

- [[neural-networks-overview]]
- [[how-neural-networks-train]]
- [[attention-mechanism]]
- [[embeddings-and-tokenization]]
- [[llm-inference-optimization]]
