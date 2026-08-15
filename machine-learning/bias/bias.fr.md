---
title: "Le biais dans les couches linéaires"
aliases: ["bias", "biais"]
tags: [machine-learning, neural-networks, fundamentals]
created: 2026-05-29
source: conversation with Claude
lang: fr
translations:
  - "[[bias.en]]"
related: ["[[normalization.fr]]", "[[how-neural-networks-train.fr]]", "[[transformer-architecture.fr]]", "[[attention-mechanism.fr]]", "[[distillation.fr]]"]
---

# Le biais dans les couches linéaires

## TL;DR

> Le biais $b$ dans $y = Wx + b$ est un terme additif appris qui **décale** la sortie de la couche linéaire. Sans lui, l'hyperplan défini par $W$ est forcé de passer par l'origine, ce qui restreint sévèrement la capacité expressive du réseau. Les Transformers modernes (Llama, Mistral) le suppriment souvent car la normalisation en amont absorbe son rôle.

## Key concepts

- **Biais ($b$)** — vecteur appris (un scalaire par neurone de sortie) ajouté après la combinaison linéaire $Wx$.
- **Rôle géométrique** — déplace l'hyperplan / le seuil d'activation. Sans biais, tous les hyperplans de séparation passent par l'origine.
- **Initialisation** — généralement à zéro. Pas problématique (contrairement aux poids) car les poids aléatoires brisent déjà la symétrie.
- **Entraînement** — exactement comme les poids, par rétropropagation : $\partial L / \partial b = \partial L / \partial z$.
- **Suppression dans les Transformers modernes** — Llama omet les biais car le [[normalization.fr|RMSNorm pre-norm]] les rend redondants.

## Deep dive

### Forme mathématique et interprétation géométrique

Dans une couche linéaire :

$$y = Wx + b$$

où $W$ est la matrice de poids et $b$ un vecteur de biais (tous deux appris). En 1D :

$$y = w \cdot x + b$$

C'est l'équation d'une droite. $w$ est la pente, $b$ est l'**ordonnée à l'origine** — où la droite coupe l'axe vertical. Sans $b$, la droite est forcée de passer par $(0, 0)$.

En $N$ dimensions, $Wx$ définit un **hyperplan passant par l'origine**. Le terme $+b$ permet à cet hyperplan de **se translater** où on veut dans l'espace.

### Pourquoi c'est nécessaire (sans normalisation)

Sans biais, $x = 0 \implies y = 0$ toujours. Le neurone est incapable d'émettre un signal quand toutes ses entrées sont nulles. Plus généralement, le réseau est contraint à ce que toutes ses transformations affines passent par l'origine — une contrainte structurelle gratuite.

**Exemple concret avec ReLU** :

- Sans biais : $\text{ReLU}(w \cdot x)$ — s'active quand $wx > 0$, donc autour de $x = 0$.
- Avec biais : $\text{ReLU}(wx + b)$ — s'active quand $wx + b > 0$, donc autour de $x = -b/w$.

Le biais déplace le **seuil d'activation** du neurone. C'est ce qui permet à chaque neurone de se spécialiser sur une zone précise de l'espace d'entrée (par exemple, "détecter quand la valeur dépasse 25" plutôt que "détecter quand la valeur dépasse 0").

### Initialisation et entraînement

**Initialisation** : zéro, typiquement.

Pourquoi ça ne pose pas le même problème que pour les poids :
- Les **poids** initialisés à zéro créent une symétrie pathologique : tous les neurones d'une couche calculent la même chose, leurs gradients sont identiques → ils ne se différencient jamais. D'où l'initialisation aléatoire (Xavier, He).
- Les **biais** initialisés à zéro ne créent pas cette symétrie, car les poids aléatoires la cassent déjà. Le biais peut donc partir de zéro sans bloquer l'apprentissage.

Variantes spécialisées :
- Biais de la dernière couche d'un classifieur : initialiser à $\log(\text{freq}_{\text{classe}})$ pour démarrer proche d'une bonne estimation.
- Biais des "forget gates" dans les LSTM : initialiser à 1 plutôt que 0 (astuce empirique).

**Entraînement** : exactement comme les poids. Si $z = Wx + b$ et qu'on connaît $\partial L / \partial z$, alors :

$$\frac{\partial L}{\partial b} = \frac{\partial L}{\partial z} \qquad \frac{\partial L}{\partial W} = \frac{\partial L}{\partial z} \cdot x^\top$$

Update standard :

$$b \leftarrow b - \eta \cdot \frac{\partial L}{\partial b}$$

Adam s'applique de la même façon (momentum + adaptation par paramètre). Le biais est un paramètre comme un autre — aucun traitement spécial dans le code des optimizers.

### La suppression du biais dans les Transformers modernes

Les modèles récents (Llama, Mistral) **n'ont aucun biais** dans leurs couches linéaires — ni dans le MLP, ni dans les projections Q/K/V/O de l'[[attention-mechanism.fr|attention]]. Pourquoi ils peuvent se le permettre :

**1. La normalisation absorbe le rôle.** Avec [[normalization.fr|LayerNorm/RMSNorm]] avant chaque sous-bloc (pre-norm), les activations sont déjà re-centrées avant la couche linéaire suivante. Un biais après serait redondant.

Démonstration pour LayerNorm : si $z = Wx + b$, alors $\text{mean}(z) = \text{mean}(Wx) + b$. La soustraction de la moyenne donne $z - \text{mean}(z) = Wx - \text{mean}(Wx)$ — **le $b$ s'annule mathématiquement**. C'est un paramètre fantôme.

**2. Économie de paramètres.** Sur 70 milliards de paramètres, supprimer les biais en économise quelques centaines de millions. Marginal en taille mais simplifie l'architecture.

**3. Empirique.** Études contrôlées : performance identique (ou légèrement meilleure) sans biais, à condition d'avoir une normalisation propre.

### Panorama actuel

| Architecture | Biais |
|---|---|
| MLP isolé, petit réseau | Présent partout — nécessaire |
| GPT-2, BERT (post-norm + LayerNorm complet) | Présent partout — convention historique |
| Llama, Mistral (pre-norm + RMSNorm) | **Supprimé** des couches linéaires |

Même quand le biais est supprimé, le $\gamma$ de RMSNorm (paramètre appris) fournit déjà une amplification par dimension, ce qui suffit à la flexibilité dans la pratique.

## Examples & analogies

- **Thermostat spécialisé.** Un neurone détecteur de "il fait chaud" voudrait s'activer autour de 25°C. Sans biais, son seuil est coincé à 0°C. Le biais lui permet de placer son seuil personnel où il veut.
- **Ordonnée à l'origine.** En géométrie scolaire : $y = ax + b$. $a$ est la pente, $b$ est où la droite coupe l'axe vertical. Sans $b$, toutes les droites passent par l'origine — gravement restrictif.

## Open questions

- Pourquoi l'initialisation à 1 (au lieu de 0) du biais des forget gates aide-t-elle si bien les LSTM ?
- Existe-t-il des architectures où réintroduire des biais serait utile malgré la normalisation ?
- Le $\gamma$ de RMSNorm compense-t-il vraiment l'absence de biais dans toutes les conditions, ou y a-t-il des cas pathologiques ?

## Related notes

- [[normalization.fr]]
- [[how-neural-networks-train.fr]]
- [[transformer-architecture.fr]]
- [[attention-mechanism.fr]]
- [[distillation.fr]]
