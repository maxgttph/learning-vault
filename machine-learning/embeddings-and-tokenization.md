---
title: "Embeddings et tokenization"
aliases: ["embedding", "tokenizer", "BPE"]
tags: [machine-learning, embeddings, tokenizer, llm]
created: 2026-05-26
source: conversation with Claude
related: ["[[transformer-architecture]]", "[[attention-mechanism]]", "[[recurrent-networks-and-lstm]]", "[[world-models]]"]
---

# Embeddings et tokenization

## TL;DR

> Le texte brut est d'abord découpé en **tokens** par un algorithme séparé (BPE), entraîné en amont du modèle. Chaque token est transformé en vecteur via une **matrice d'embedding** apprise par rétropropagation — personne ne décide à la main quel vecteur correspond à quel mot. À la sortie, un **un-embedding** reprojette sur le vocabulaire pour produire des probabilités de token suivant.

## Key concepts

- **Token** — unité de base manipulée par un LLM, typiquement un sous-mot (entre une lettre et un mot complet).
- **Tokenizer** — algorithme qui découpe le texte brut. Entraîné séparément, en amont du modèle. Figé ensuite.
- **BPE (Byte Pair Encoding)** — fusionne itérativement les paires de caractères les plus fréquentes. Utilisé par GPT, Llama.
- **Matrice d'embedding** — table de lookup $|\text{vocab}| \times d_{\text{model}}$, **apprise** comme tous les autres poids.
- **Un-embedding** — matrice $d_{\text{model}} \times |\text{vocab}|$ qui projette la sortie du modèle sur les scores du vocabulaire. Souvent partagée avec l'embedding.

## Deep dive

### La tokenization vient avant tout

Le texte brut est d'abord découpé en tokens. Les deux algos dominants :

- **BPE (Byte Pair Encoding)** — utilisé par GPT, Llama. Part de caractères individuels et fusionne itérativement les paires les plus fréquentes, jusqu'à atteindre la taille de vocabulaire cible (typiquement 30 000 à 200 000 tokens).
- **SentencePiece / Unigram** — utilisé par certains modèles Google.

Le tokenizer est entraîné AVANT le modèle, séparément, à partir du corpus. Une fois figé, il devient indissociable du modèle : changer le tokenizer = tout recommencer.

### L'embedding est juste un lookup

Une matrice $E$ de taille $|\text{vocab}| \times d_{\text{model}}$. Pour $d_{\text{model}} = 8192$ et 128k de vocabulaire : ~1 milliard de paramètres rien que pour cette matrice.

Le token d'ID 42 → ligne 42 de $E$. **Pas de calcul, juste de la lecture.**

**Point crucial** : personne ne décide qui correspond à quoi.
- La matrice est **initialisée aléatoirement** (gaussienne $\mathcal{N}(0,\ 0.02)$).
- Elle est **entraînée par rétropropagation** comme tous les autres poids.

À la fin de l'entraînement, des propriétés émergent automatiquement :
- Mots de contextes similaires → vecteurs géométriquement proches.
- $\text{Paris} - \text{France} \approx \text{Berlin} - \text{Allemagne}$ (arithmétique vectorielle célèbre).

C'est de l'auto-organisation pure, dirigée par la pression d'optimisation de la loss (prédire le token suivant).

### L'un-embedding ferme la boucle

À la fin du forward pass, on a un vecteur de dimension $d_{\text{model}}$ (la représentation finale du dernier token). On le multiplie par une matrice $d_{\text{model}} \times |\text{vocab}|$, ce qui donne un **logit par token possible** dans le vocabulaire. Un softmax transforme ça en distribution de probabilités → on échantillonne le prochain token.

**Astuce courante** : beaucoup de modèles (GPT-2, certains Llama) **partagent** les poids entre embedding et un-embedding ($E$ et $E^\top$). Économise des paramètres et a une justification théorique (symétrie de l'opération encoder/decoder de tokens).

### Pourquoi un changement de tokenizer casse tout

Les IDs de tokens sont **arbitraires** — purement définis par le tokenizer. Si on change :

- **Le tokenizer entier** → les IDs changent, donc la matrice d'embedding entière devient absurde, donc tout le modèle aussi. Il faut **tout réentraîner**.
- **Seulement la matrice d'embedding** (même tokenizer) → le reste du modèle ne sait plus à quoi correspondent les vecteurs en entrée. À minima, réentraîner les couches voisines, en pratique tout le modèle.

C'est pour ça qu'un téléchargement de Llama inclut toujours son tokenizer attaché : ils forment un couple indissociable.

### Position encoding (parenthèse)

Sans information de position, l'attention traiterait "le chat dort" et "dort le chat" identiquement. On ajoute donc une **encodage de position** à l'embedding. Llama et Mistral utilisent **RoPE** (Rotary Position Embedding), qui encode la position via une rotation appliquée aux vecteurs Q et K dans [[attention-mechanism|l'attention]].

## Examples & analogies

- **Index dans un dictionnaire géant.** L'embedding n'apprend pas "ce qu'est un chat" — il apprend à quel point de l'espace géométrique placer le vecteur "chat" pour que les couches suivantes le manipulent correctement.
- **Couple tokenizer / modèle.** Comparable à une serrure et sa clé : la clé (modèle) n'a aucun sens sans la serrure (tokenizer) qui définit la géométrie attendue. Distribuer l'un sans l'autre est inutilisable.

## Open questions

- Pourquoi le partage de poids embedding ↔ un-embedding fonctionne-t-il en pratique ? (Beaucoup de modèles le font, mais ce n'est pas une nécessité théorique.)
- Quels gains apporteraient des tokenizers adaptatifs ou contextuels ?
- Comment gérer proprement les langues sous-représentées dans le corpus de tokenization (qui finissent éclatées en beaucoup de tokens) ?

## Related notes

- [[transformer-architecture]]
- [[attention-mechanism]]
- [[recurrent-networks-and-lstm]]
- [[world-models]]
