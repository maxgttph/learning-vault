---
title: "Le mécanisme d'attention (Q, K, V, O)"
aliases: ["attention", "self-attention", "multi-head attention"]
tags: [machine-learning, attention, transformer, llm]
created: 2026-05-26
source: conversation with Claude
lang: fr
translations:
  - "[[attention-mechanism.en]]"
related: ["[[transformer-architecture.fr]]", "[[embeddings-and-tokenization.fr]]", "[[llm-inference-optimization.fr]]", "[[how-neural-networks-train.fr]]", "[[normalization.fr]]", "[[bias.fr]]", "[[recurrent-networks-and-lstm.fr]]", "[[mixture-of-experts.fr]]"]
---

# Le mécanisme d'attention (Q, K, V, O)

## TL;DR

> L'attention est un **routage différentiable d'information entre tokens**. Trois matrices apprises **W_Q, W_K, W_V** projettent chaque token dans trois espaces (question / étiquette / contenu) ; une quatrième **W_O** post-traite le résultat. Point crucial : ces matrices sont **mathématiquement identiques au départ** — leurs rôles distincts émergent uniquement de leur **position dans la formule**, et de la pression du gradient qui les spécialise pendant l'entraînement.

## Key concepts

### Matrices apprises (paramètres figés après entraînement)

- **W_Q, W_K, W_V** — trois matrices $d_{\text{model}} \times d_k$ qui projettent chaque embedding dans trois espaces différents. Identiques par nature (même forme, même initialisation, même algorithme de mise à jour). Leurs rôles distincts viennent de leur **position dans la formule**, pas d'une propriété intrinsèque.
- **W_O** — quatrième matrice (typiquement $d_{\text{model}} \times d_{\text{model}}$). Joue dans une équipe à part : n'intervient **pas** dans le routage, seulement dans le post-traitement.

### Vecteurs calculés à la volée (activations transitoires)

- **Q, K, V** — pour chaque token, vecteurs obtenus en appliquant les matrices $W_*$ à son embedding. $Q$ = "ce que je cherche", $K$ = "mon étiquette publique", $V$ = "ce que je transmets".
- **Scores et A** — matrice $n \times n$ des similarités entre chaque paire de tokens, puis sa normalisation en distribution.

### Concepts supplémentaires

- **Masque causal** — interdit aux tokens de regarder le futur. Essentiel à la prédiction autorégressive.
- **Multi-head attention** — plusieurs attentions en parallèle (typ. 8 à 64 têtes), chacune capturant un type de relation différent.

## Deep dive

### Le piège terminologique : matrices vs vecteurs

"Q, K, V" désigne deux choses différentes selon le contexte :

| Notation | C'est quoi | Statut |
|---|---|---|
| $W_Q$, $W_K$, $W_V$, $W_O$ | Matrices de poids apprises | Paramètres figés après entraînement |
| $Q$, $K$, $V$ | Vecteurs calculés à la volée pour chaque token | Activations transitoires, recalculées à chaque inférence |

Les $W_*$ vivent dans le fichier `.safetensors`. Les $Q, K, V$ n'existent qu'à l'instant où le token traverse la couche. À retenir : **les matrices sont les paramètres, les vecteurs sont les activations**.

### Analogie de la bibliothèque

Tu arrives avec une question dans la tête (Q). Chaque livre a une étiquette sur la tranche (K) et un contenu à l'intérieur (V). Tu compares ta Q à chaque K et tu repars avec un **mélange pondéré** du contenu V des livres — beaucoup de celui dont l'étiquette matche, peu (ou rien) des autres.

L'attention fait exactement ça, sauf que :
- Chaque token est simultanément lecteur ET livre.
- La comparaison est différentiable (produit scalaire + softmax).
- Tout est appris par descente de gradient.

### La recette mathématique pas-à-pas

Pour une séquence $X$ (chaque ligne = un token) :

**Étape 1 — projeter** (3 matrices en parallèle, un vecteur Q/K/V par token) :

$$Q = X\, W_Q \qquad K = X\, W_K \qquad V = X\, W_V$$

**Étape 2 — scores de similarité** (matrice $n \times n$) :

$$\text{scores} = \frac{Q\, K^\top}{\sqrt{d_k}}$$

où $\text{scores}_{ij}$ mesure « à quel point le token $i$ s'intéresse au token $j$ ».

**Étape 3 — masque causal + softmax** (chaque ligne devient une distribution) :

$$A = \text{softmax}(\text{scores} + \text{masque})$$

**Étape 4 — mélange pondéré des contenus** (chaque ligne $i$ = combinaison des $V_j$ pondérée par $A_{ij}$) :

$$\text{mixed} = A\, V$$

**Étape 5 — projection finale** (post-traitement, retour vers $d_{\text{model}}$) :

$$\text{out} = \text{mixed} \cdot W_O$$

Sous forme compacte, c'est la formule canonique de *Attention Is All You Need* :

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{Q\, K^\top}{\sqrt{d_k}}\right) V$$

### Pourquoi les rôles diffèrent si les matrices sont identiques

C'est la question profonde du mécanisme. Si rien ne distingue intrinsèquement W_Q de W_K de W_V (même forme, même initialisation, même update), pourquoi finissent-elles avec des rôles si différents ?

Réponse : **deux mécanismes complémentaires**.

**1. L'asymétrie structurelle (position dans la formule)**

- **W_Q vs W_K** : leurs sorties se rencontrent dans $Q\, K^\top$. Mais Q est à gauche, K transposée à droite. Conséquence :

  $$\text{score}_{ij} = Q_i \cdot K_j$$

  Ce produit **n'est pas symétrique** : $\text{score}_{ij} \neq \text{score}_{ji}$ en général. D'où la distinction "qui écoute / qui est écouté" — encodée dans la position **gauche/droite** du produit, pas dans les matrices.

- **W_V vs W_Q/W_K** : la sortie de W_V n'apparaît **qu'après** le softmax, multipliée par A. Elle ne participe pas du tout au calcul des scores. C'est cette position-là qui en fait "le contenu transmis" et non "le filtre du routage".

**2. La pression du gradient (spécialisation forcée par l'entraînement)**

Pendant l'entraînement, chaque matrice reçoit son gradient via le chemin où sa sortie est utilisée :

- $W_V$ reçoit $\text{Loss} \to \text{mixed} = A V \to V \to W_V$.
  Message du gradient : *"modifie-toi pour que le contenu mélangé soit utile en sortie."*

- $W_Q$ reçoit $\text{Loss} \to \text{mixed} \to A \to \text{softmax} \to \text{scores} \to Q \to W_Q$.
  Message du gradient : *"modifie-toi pour que les scores produisent un meilleur routage."*

- $W_K$ : idem que $W_Q$, par le chemin symétrique côté K.

Les matrices partent identiques (du bruit gaussien). Mais comme la pression de gradient est différente sur chacune, **elles divergent par spécialisation forcée**. Au jour 0, indiscernables. Au jour N, expertes dans leur rôle structurel.

**Test mental** : si on permutait les rôles W_V ↔ W_Q dans la formule avant l'entraînement (sans changer les matrices initiales), le système marcherait pareil. À la fin, la matrice qu'on aurait appelée W_V ressemblerait à ce qu'on appelait W_Q, et inversement. **Le contenu appris dépend entièrement de la position où la matrice est branchée.**

### Pourquoi W_O n'est pas sur le même plan que Q/K/V

Techniquement, W_O est une matrice apprise comme les trois autres. Mais conceptuellement :

- $W_Q, W_K, W_V$ **participent au mécanisme d'attention proprement dit** — elles décident qui écoute qui et avec quel contenu.
- $W_O$ est **après** l'attention. Si on remplaçait $W_O$ par l'identité, l'attention fonctionnerait toujours (les tokens s'écouteraient correctement), juste le résultat sortirait dans un espace non recombiné. Alors que sans $W_V$, il n'y aurait littéralement rien à transmettre. Sans $W_Q$ ou $W_K$, le routage serait cassé.

Partition plus honnête :
- **Phase de routage** : W_Q, W_K, W_V
- **Phase d'intégration** : W_O

W_O joue principalement deux rôles :
1. **Recoller les têtes** en multi-head — les sorties des N têtes concaténées sont mélangées entre elles par W_O.
2. **Reprojeter** dans la dimension $d_{\text{model}}$ du flux résiduel.

La convention "Q, K, V, O" est gardée car les 4 sont structurellement similaires en code, mais il est utile de savoir que W_O joue dans une équipe différente.

### L'attention ≠ co-occurrence statistique

Piège classique : penser que l'attention apprend "les mots qui apparaissent souvent ensemble". Faux.

- La **co-occurrence statistique** est capturée par les [[embeddings-and-tokenization.fr|embeddings]] (cf. word2vec).
- L'**attention** apprend un **routage contextuel** : "étant donné l'état de chaque token (déjà contextualisé), quels autres tokens contiennent l'info dont j'ai besoin pour me transformer utilement ?"

Exemple : dans "Le chat que Marie a adopté hier dort", quand le modèle traite "dort", il a besoin de retrouver le **sujet syntaxique** ("chat", sept mots plus tôt). C'est une relation grammaticale, pas une corrélation statistique.

### Multi-head : plusieurs spécialistes en parallèle

Pour Llama 3 70B : $d_{\text{model}} = 8192$, 64 têtes de dimension $d_k = 128$. Chaque tête a ses propres W_Q, W_K, W_V, et apprend un type de relation différent : références anaphoriques, syntaxe sujet-verbe, négation, contrastes, etc. Les sorties des têtes sont concaténées, puis W_O les recombine.

Une couche d'attention contient donc 4 matrices $8192 \times 8192 \approx 270\text{M}$ paramètres. Sur 80 couches $\approx 21$ milliards pour l'attention seule.

## Examples & analogies

- **Bibliothèque universelle** : chaque token est simultanément lecteur (Q) et livre (K, V). Tout le monde lit tout le monde en une étape parallèle.
- **Trois ouvriers identiques.** Embauche trois ouvriers avec la même formation, poste l'un à la réception, l'autre au tri, le troisième à l'expédition. Au bout d'un an, ils sont des experts radicalement différents — pas par nature, mais parce que la position dans la chaîne de production a façonné leur expertise. C'est exactement ce qui arrive à W_Q, W_K, W_V pendant l'entraînement.
- **Analogie neuroscientifique.** Un neurone du cortex visuel n'a rien d'intrinsèquement "visuel" — il l'est devenu parce qu'il est branché en aval de la rétine. Les expériences de Mriganka Sur (années 90) ont montré qu'en rebranchant ce neurone à l'oreille interne, il devient auditif. Le sens émerge de la structure du graphe, pas du contenu intrinsèque.

## Open questions

- Comment interpréter mécaniquement ce que chaque tête a appris ? (mechanistic interpretability)
- Quelles alternatives à l'attention quadratique sont viables ? (linear attention, sliding window, sparse, state-space)
- Pourquoi le multi-head fonctionne-t-il mieux qu'une seule grosse tête ? Régularisation ? Diversité des routages appris ?
- Que se passe-t-il si on initialise $W_Q = W_K = W_V$ à des valeurs **exactement identiques** (pas juste de même distribution) ? La symétrie est-elle brisée par le bruit du SGD au premier pas ?

## Related notes

- [[transformer-architecture.fr]]
- [[embeddings-and-tokenization.fr]]
- [[llm-inference-optimization.fr]]
- [[how-neural-networks-train.fr]]
- [[normalization.fr]]
- [[bias.fr]]
- [[recurrent-networks-and-lstm.fr]]
- [[mixture-of-experts.fr]]
