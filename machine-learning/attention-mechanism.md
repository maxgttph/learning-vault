---
title: "Le mécanisme d'attention (Q, K, V, O)"
aliases: ["attention", "self-attention", "multi-head attention"]
tags: [machine-learning, attention, transformer, llm]
created: 2026-05-26
source: conversation with Claude
related: ["[[transformer-architecture]]", "[[embeddings-and-tokenization]]", "[[llm-inference-optimization]]", "[[how-neural-networks-train]]"]
---

# Le mécanisme d'attention (Q, K, V, O)

## TL;DR

> L'attention est un **routage différentiable d'information entre tokens**. Chaque token pose une question (**Query**), affiche une étiquette (**Key**), transmet un contenu (**Value**). Les correspondances Q·K déterminent un mélange pondéré de V, recombiné par une matrice de sortie **W_O**. Tout est appris par rétropropagation.

## Key concepts

- **Query (Q)** — projection apprise d'un token : "ce que je cherche dans les autres".
- **Key (K)** — projection : "mon étiquette par laquelle je peux être trouvé".
- **Value (V)** — projection : "ce que je transmets quand on m'écoute".
- **W_O** — matrice de sortie qui recolle les têtes multiples et reprojette dans l'espace résiduel.
- **Masque causal** — interdit aux tokens de regarder le futur. Essentiel à la prédiction autorégressive.
- **Multi-head attention** — plusieurs attentions en parallèle (typ. 8 à 64 têtes), chacune capturant un type de relation différent.

## Deep dive

### Analogie de la bibliothèque

Tu arrives avec une question dans la tête (Q). Chaque livre a une étiquette sur la tranche (K) et un contenu à l'intérieur (V). Tu compares ta Q à chaque K et tu repars avec un **mélange pondéré** du contenu V des livres — beaucoup du livre dont l'étiquette matche bien ta question, peu (ou rien) des autres.

L'attention fait exactement ça, sauf que :
- Chaque token est simultanément lecteur ET livre.
- La comparaison est différentiable (produit scalaire + softmax).
- Tout est appris par descente de gradient.

### La recette mathématique

Pour une séquence `X` (chaque ligne = un token) :

```
Q = X · W_Q
K = X · W_K
V = X · W_V

scores       = (Q · Kᵀ) / √d_k          ← scaling pour stabilité
scores_mask  = scores + masque causal    ← −∞ sur le futur
A            = softmax(scores_mask)      ← chaque ligne = distribution
out_pré_O    = A · V                     ← mélange pondéré
out          = out_pré_O · W_O           ← projection finale
```

`W_Q`, `W_K`, `W_V`, `W_O` sont des **paramètres entraînés** — pas codés à la main. Voir [[how-neural-networks-train]] pour la mécanique d'entraînement, qui s'applique ici comme à tout le reste du réseau.

### Pourquoi V est calculé séparément de K

Pour décorréler deux rôles différents :

- **K** = "comment je peux être trouvé"
- **V** = "ce que je transmets quand on me trouve"

Un mot peut avoir une signature de recherche utile (K) tout en transmettant un contenu différent (V). Confondre les deux force le modèle à utiliser la même représentation pour les deux rôles → perte de flexibilité.

### Pourquoi W_O existe

Trois raisons :

1. **Recoller les têtes** en multi-head. Les sorties de N têtes concaténées forment un long vecteur juxtaposé sans mélange. W_O combine ces composantes.
2. **Reprojeter** dans la dimension du flux résiduel (qui doit matcher `d_model`).
3. **Capacité expressive** supplémentaire (un degré de liberté de plus dans la transformation).

### L'attention ≠ co-occurrence

Piège classique : penser que l'attention apprend "les mots qui apparaissent souvent ensemble". Faux.

- La **co-occurrence statistique** est capturée par les [[embeddings-and-tokenization|embeddings]] (cf. word2vec).
- L'**attention** apprend un **routage contextuel** : "étant donné l'état de chaque token (déjà contextualisé), quels autres tokens contiennent l'info dont j'ai besoin pour me transformer utilement ?"

Exemple : dans "Le chat que Marie a adopté hier dort", quand le modèle traite "dort", il a besoin de récupérer le **sujet syntaxique** ("chat", sept mots plus tôt). C'est une relation grammaticale, pas une corrélation statistique.

### Multi-head : plusieurs spécialistes en parallèle

Pour Llama 3 70B : `d_model = 8192`, 64 têtes de dimension `d_k = 128`. Chaque tête a ses propres W_Q, W_K, W_V, et apprend un type de relation différent : références anaphoriques (pronoms → antécédents), syntaxe sujet-verbe, négation, contrastes, etc.

Une couche d'attention contient donc 4 matrices `8192 × 8192` ≈ 270M paramètres. Sur 80 couches ≈ 21 milliards pour l'attention seule.

## Examples & analogies

- **Bibliothèque universelle** : chaque token est à la fois lecteur (Q) et livre (K, V), et tout le monde lit tout le monde en une seule étape parallèle.
- **Routage logistique** : Q comme adresse de demande, K comme catalogue d'expéditeurs, V comme colis transmis, A comme matrice de transport (combien d'unités du token j vers le token i).

## Open questions

- Comment interpréter mécaniquement ce que chaque tête a appris ? (mechanistic interpretability)
- Quelles alternatives à l'attention quadratique sont viables sur le long terme ? (linear attention, sliding window, sparse, state-space)
- Pourquoi le multi-head fonctionne-t-il mieux qu'une seule grosse tête ? (réponse partielle : régularisation et capacité d'apprentissage de routages distincts)

## Related notes

- [[transformer-architecture]]
- [[embeddings-and-tokenization]]
- [[llm-inference-optimization]]
- [[how-neural-networks-train]]
