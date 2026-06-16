---
title: "La distillation des LLM"
aliases: ["distillation", "knowledge distillation", "distillation de connaissance"]
tags: [machine-learning, llm, distillation, training, post-training]
created: 2026-06-16
source: conversation with Claude
related: ["[[how-neural-networks-train]]", "[[embeddings-and-tokenization]]", "[[bias]]", "[[llm-inference-optimization]]", "[[transformer-architecture]]", "[[mixture-of-experts]]"]
---

# La distillation des LLM

## TL;DR

> La distillation entraîne un petit modèle **élève** à imiter un grand modèle **professeur**, pour transférer sa compétence dans un réseau moins coûteux. Le signal clé n'est pas la "bonne réponse" mais la **distribution complète** du professeur sur le vocabulaire (les *soft targets*) — qui encode bien plus d'information qu'un simple token cible.

## Key concepts

- **Logits** — scores bruts produits par l'un-embedding, un par token du vocabulaire, avant le softmax. Réels non bornés ; seules leurs **différences** comptent.
- **Soft targets / dark knowledge** — la distribution de probabilités du professeur (pas juste son top-1). Encode *quelles mauvaises réponses sont plausibles*.
- **Température $T$** — facteur appliqué aux logits avant softmax ($z_i / T$) pour aplatir la distribution et rendre visibles les petites probabilités.
- **Logit distillation** — l'élève imite la distribution softmaxée du professeur (perte KL). Exige l'accès aux logits du professeur.
- **Sequence-level distillation** — le professeur *génère* du texte, l'élève est fine-tuné dessus en next-token classique. Pas besoin des logits. Méthode dominante en pratique.
- **Plafond (ceiling)** — l'élève converge vers le **min** entre la capacité du professeur et sa propre capacité ; jamais au-delà.

## Deep dive

### Où se situe la distillation dans l'entraînement

Le mot *entraînement* est un parapluie, pas une phase : toute mise à jour de poids via la boucle forward → loss → backprop → optimizer (voir [[how-neural-networks-train]]). Ce qui distingue les phases, c'est **les données et la loss**.

- **Pre-training** (~90 % du compute) — next-token prediction sur du texte brut massif (Llama 3 : 15 000 milliards de tokens), sans annotation. Construit la **connaissance** et la compétence linguistique.
- **Post-training** — tout ce qui suit. N'ajoute presque pas de connaissance ; **sculpte le comportement** à partir de ce que le pre-training a appris :
  - **SFT** — fine-tuning sur paires `(instruction, bonne réponse)`, même loss next-token. Apprend le format "assistant".
  - **Alignement (RLHF / DPO)** — loss de préférence (pas next-token), à partir de classements de réponses.
  - **Distillation** — transfère le *comportement* du professeur. Structurellement proche du SFT (si on fine-tune sur du texte généré) ou objectif distinct (si on matche les logits avec KL + température).

Modèle mental : **le pre-training verse la connaissance, le post-training la sculpte en forme utilisable.** La distillation sculpte l'élève pour qu'il ressemble au professeur.

### Pourquoi les soft targets valent plus que les hard labels

La loss standard d'un LLM est une cross-entropy contre **le vrai token suivant** — une seule bonne réponse ("hard label"). Pour "La capitale de la France est ___", la cible est `Paris`, probabilité 1, tout le reste 0.

Mais un professeur entraîné produit une **distribution complète** via ses logits (voir l'un-embedding dans [[embeddings-and-tokenization]]) :

```
Paris 0.90, Lyon 0.04, le 0.02, Marseille 0.01, banane 1e-7, ...
```

Cette distribution dit *quelles erreurs sont des quasi-erreurs* (`Lyon` plausible, `banane` absurde). C'est cette structure — le **dark knowledge** — qu'on veut transférer à l'élève.

### Le rôle de la température

Problème : un professeur confiant met presque toute la masse sur `Paris` (0.90+), écrasant l'information intéressante (`Lyon` 0.04 vs `banane` 1e-7) près de zéro. Dans la loss, ces différences minuscules ne pèsent presque rien — le dark knowledge est gaspillé.

La **température aplatit la distribution** pour rendre ces petites probabilités exploitables. On divise chaque logit par $T$ avant le softmax :

$$p_i = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$$

- $T = 1$ → softmax normal.
- $T > 1$ (ex. 3–4) → réduit les **écarts** entre logits → distribution plus plate. `Paris` passe de 0.90 à ~0.50, `Lyon` de 0.04 à ~0.12, `banane` de 1e-7 à ~1e-4. La structure relative est amplifiée et apprenable.
- $T \to \infty$ → distribution uniforme. $T < 1$ → plus piquée.

Rappel : le softmax ne dépend que des **différences** de logits. Diviser par $T = 3$ réduit chaque écart d'un facteur 3 — exactement "rendre la distribution moins pointue".

On entraîne professeur **et** élève à la même température, et la perte de distillation rapproche la distribution de l'élève de celle du professeur (divergence KL). Souvent combinée à la hard-label cross-entropy :

$$L = \alpha \cdot L_{\text{distill}}(T) \cdot T^2 + (1 - \alpha) \cdot L_{\text{CE}}(\text{vrai token})$$

Le terme $T^2$ recale les gradients que l'aplatissement réduit. Après entraînement, on jette la température et l'élève tourne à $T = 1$.

> **Nuance importante** : cette méthode (*logit distillation*) exige l'accès aux logits du professeur. Beaucoup de modèles "distillés" actuels font plutôt de la **sequence-level distillation** : le professeur *génère* du texte, l'élève est fine-tuné dessus en next-token classique (pas de température, pas de KL). C'est de l'entraînement sur données synthétiques.

### Convergence : l'élève rejoint-il le professeur ?

Principe : la sortie du professeur est le **seul signal**, donc le professeur est un **plafond** — l'élève converge *vers* lui, jamais *au-delà* (en distillation pure). La capacité de l'élève est un second plafond. L'élève tend vers **le plus bas des deux**.

- **Élève de même taille** — en principe il peut approcher le professeur (capacité suffisante pour représenter la même fonction), sur la distribution de prompts échantillonnée. Mais il *matche* au mieux, ne dépasse jamais ; et diverge hors distribution. Rarement utile en pratique.
- **Élève plus petit** (cas normal) — **plateau strictement sous le professeur.** Sa capacité limitée est le plafond contraignant. Plus de tokens aident à *atteindre* le plateau (rendements log-décroissants), puis ne servent plus. Un 7B distillé d'un 70B referme une *partie* de l'écart, pas tout.
- **Élève plus grand** — converge vers le professeur **et s'arrête là** : la capacité excédentaire n'achète rien, car le professeur est le plafond. Pire, le surplus peut **sur-apprendre les défauts et erreurs** du professeur (il hérite de ses biais, voir [[bias]]). Pour *dépasser* le professeur, il faut un signal au-delà de lui : ground-truth, RL, auto-amélioration.

Résumé : précision $\to \min(\text{plafond professeur},\ \text{plafond capacité élève})$.

### Pourquoi partir d'un modèle de base déjà pré-entraîné

Intuition fréquente : « si la backprop vient des sorties du professeur, le modèle de base ne fait qu'accélérer, pas rendre plus intelligent. » Vraie dans **une limite**, fausse dans **le régime qui compte**.

- **Limite à données infinies** : avec une donnée professeur infinie et couvrante, le point de départ s'efface — la descente de gradient sculpterait la même fonction depuis des poids aléatoires. Là, la base ne fait gagner que du temps.
- **Régime réel (données finies)** : un jeu de distillation fait ~1 milliard de tokens / ~1M exemples, soit **des milliers de fois moins** que le pre-training (15T). Ce signal est bien trop creux pour reconstruire de zéro la connaissance générale. Le professeur ne montre son comportement que sur les prompts échantillonnés ; partout ailleurs, un élève initialisé aléatoirement serait incompétent. **La base comble exactement ces trous** avec sa propre connaissance de pre-training.

Donc : compétence finale = (connaissance pré-entraînée de l'élève) + (comportement distillé du professeur). Le premier terme est énorme et irremplaçable à l'échelle de données réelle → la base est **porteuse, pas un simple accélérateur**. C'est pourquoi un meilleur modèle de base donne un meilleur élève à donnée professeur égale — ce qui serait faux si la base ne faisait que gagner du temps.

### Combien de tokens pour des performances proches ?

Pas de chiffre unique, mais des ordres de grandeur :

- Bien plus petit que le pre-training : typiquement **~100k à ~1M exemples** (centaines de millions à quelques milliards de tokens). DeepSeek-R1 : ~800k échantillons générés.
- On *matche* rarement le professeur à budget égal — la distillation fait qu'un petit modèle frappe au-dessus de sa catégorie, sans tout combler.
- Rendements décroissants rapides (forme en log) : les premiers 100k exemples rapportent beaucoup, le reste de moins en moins. La **diversité/couverture** des prompts compte plus que le volume brut.

Le modèle de base est souvent le facteur dominant : capacité minimale (plancher), connaissance déjà présente, alignement professeur–élève (architecture, **tokenizer** — un tokenizer différent casse la logit distillation, voir [[embeddings-and-tokenization]]), et plafond/biais hérités.

### Distillation vs autres compressions

- **Quantization** — réduit la précision numérique des poids (voir [[llm-inference-optimization]]). Même architecture, moins de bits.
- **Pruning** — supprime poids/neurones/têtes.
- **Distillation** — entraîne un *autre* modèle, plus petit, à imiter le comportement.

Complémentaires : on distille souvent *puis* on quantize.

## Examples & analogies

- **DistilBERT** — ~40 % plus petit, ~60 % plus rapide, ~97 % des performances de BERT.
- **DeepSeek-R1 distillé** — traces de raisonnement d'un gros professeur R1 utilisées pour fine-tuner de petits élèves Qwen/Llama, transférant la capacité de chaîne de pensée.
- **Soft targets = corrigé annoté.** Un hard label dit juste "la réponse est Paris" ; les soft targets, c'est le professeur qui annote aussi *pourquoi Lyon est une erreur compréhensible et banane une absurdité*. L'élève apprend la nuance, pas que la réponse.
- **Base pré-entraînée = culture générale de l'élève.** La distillation n'est qu'une fine couche comportementale par-dessus ; sans la base, le signal du professeur est trop creux pour reconstruire le savoir.

## Open questions

- Quand préférer la logit distillation (KL + température) à la sequence-level (données synthétiques) ? Compromis accès-aux-logits / qualité.
- **On-policy distillation** : corriger les propres échantillons de l'élève par le professeur — quels gains vs distillation classique ?
- Comment dépasser le plafond du professeur ? (combiner distillation + RL + ground-truth).
- Jusqu'où la distillation propage-t-elle les biais du professeur, et comment les filtrer ?

## Related notes

- [[how-neural-networks-train]]
- [[embeddings-and-tokenization]]
- [[bias]]
- [[llm-inference-optimization]]
- [[transformer-architecture]]
- [[mixture-of-experts]]
</content>
</invoke>
