---
title: "Normalisation (LayerNorm, RMSNorm)"
aliases: ["LayerNorm", "RMSNorm", "normalisation", "pre-norm"]
tags: [machine-learning, neural-networks, transformer, normalization]
created: 2026-05-29
source: conversation with Claude
related: ["[[bias]]", "[[transformer-architecture]]", "[[attention-mechanism]]", "[[how-neural-networks-train]]"]
---

# Normalisation (LayerNorm, RMSNorm)

## TL;DR

> La normalisation force les activations à rester dans une plage contrôlée à chaque couche, empêchant l'explosion ou l'extinction du signal et des gradients dans les réseaux profonds. Sans elle, on plafonne à ~15 couches ; avec elle, 200+ couches sont entraînables. Le standard moderne est **RMSNorm + pre-norm**, plus simple et plus stable que la version originale (LayerNorm + post-norm).

## Key concepts

- **LayerNorm** — normalise un vecteur d'activations à mean 0, variance 1, puis applique une transformation affine apprise $\gamma \hat{x} + \beta$.
- **RMSNorm** — version simplifiée : rescale par la RMS sans re-centrage ni biais. Aussi performante en pratique, plus rapide.
- **Pre-norm vs post-norm** — normaliser avant le sous-bloc (pre-norm, stable en profondeur) vs après (post-norm, instable au-delà de ~12 couches).
- **Absorption du biais** — la normalisation par re-centrage annule mathématiquement tout [[bias|biais]] ajouté avant elle, rendant ces biais redondants.
- **Lissage de la loss landscape** — bénéfice empirique principal : la normalisation rend l'entraînement plus stable et tolère des learning rates plus grands.

## Deep dive

### Le problème : la dérive des activations

Sans normalisation, les activations d'un réseau profond ont tendance à **dériver** au fil des couches : soit elles **grossissent** exponentiellement (chaque couche multiplie en moyenne par $> 1$), soit elles **rétrécissent** exponentiellement. Sur 80 couches, un facteur multiplicatif de 1.05 par couche donne $1.05^{80} \approx 50$ — déjà problématique. C'est encore pire pour les gradients qui repassent par toutes ces couches en backward (vanishing/exploding gradients).

La normalisation force la stabilité du signal à chaque étape :

1. Stabilise les **activations** dans le forward.
2. Stabilise le **gradient** dans le backward.
3. **Lisse le paysage de la loss** (résultat empirique de *How does batch normalization help optimization?*, Santurkar et al. 2018 : c'est le mécanisme réel, pas la réduction du "covariate shift" comme on le croyait avant).
4. Permet des **learning rates plus grands** → entraînement plus rapide.

### LayerNorm en détail

Pour un vecteur d'activations $x \in \mathbb{R}^d$ (un seul token, dimension $d_{\text{model}}$) :

**Étape 1 — re-centrer et normaliser** :

$$\mu = \frac{1}{d}\sum_{i=1}^{d} x_i \qquad \sigma^2 = \frac{1}{d}\sum_{i=1}^{d} (x_i - \mu)^2$$

$$\hat{x}_i = \frac{x_i - \mu}{\sqrt{\sigma^2 + \varepsilon}}$$

Après cette étape, le vecteur a **moyenne 0 et variance 1**, peu importe ce qu'il était avant. Le $\varepsilon \sim 10^{-5}$ évite la division par zéro.

**Étape 2 — transformation affine apprise** :

$$y_i = \gamma_i \cdot \hat{x}_i + \beta_i$$

Avec deux vecteurs appris :
- $\gamma$ (gain) → rescale chaque dimension indépendamment.
- $\beta$ (biais de la norme) → décale chaque dimension indépendamment.

**Pourquoi cette étape 2 ?** Sans elle, on forcerait toutes les activations à mean 0, var 1 — ce qui détruirait l'information utile que le réseau aurait pu vouloir mettre à des échelles différentes. Avec $\gamma$ et $\beta$, le réseau peut **annuler** la normalisation s'il veut ($\gamma = \sigma, \beta = \mu$), la **garder telle quelle** ($\gamma = 1, \beta = 0$), ou n'importe quoi entre les deux. La normalisation devient une option modulable, pas une contrainte rigide.

Coût : $2d$ paramètres par couche LayerNorm. Pour un Transformer avec $d_{\text{model}} = 8192$ et ~200 LayerNorms : ~3.3M paramètres. Négligeable.

### RMSNorm : la simplification moderne

Utilisé par Llama, Mistral, et la plupart des modèles récents. C'est LayerNorm **sans le re-centrage et sans le biais** :

$$\text{rms}(x) = \sqrt{\frac{1}{d}\sum_{i=1}^{d} x_i^2}$$

$$y_i = \gamma_i \cdot \frac{x_i}{\text{rms}(x) + \varepsilon}$$

Différences avec LayerNorm :
- **Pas de calcul de moyenne** → un peu plus rapide (moins de réductions).
- **Pas de soustraction** $x_i - \mu$ → on ne re-centre pas, on rescale seulement.
- **Pas de biais** $\beta$ → moins de paramètres.

Pourquoi ça marche aussi bien ? Constat empirique (Zhang & Sennrich, *Root Mean Square Layer Normalization*, 2019) : le bénéfice de LayerNorm vient principalement du **rescaling**, pas du re-centrage. Une fois la norme contrôlée, le centrage est marginal. Résultat : ~7-10 % plus rapide à calculer pour une qualité identique.

### Pre-norm vs post-norm — un détail crucial

Le Transformer original (2017) plaçait LayerNorm **après** chaque sous-bloc — **post-norm** :

$$x_{\text{out}} = \text{LayerNorm}(x_{\text{in}} + \text{sublayer}(x_{\text{in}}))$$

Mais empiriquement, c'est **instable** au-delà de ~12 couches (warmup délicat requis).

Les modèles modernes utilisent **pre-norm** : normalisation **avant** le sous-bloc :

$$x_{\text{out}} = x_{\text{in}} + \text{sublayer}(\text{LayerNorm}(x_{\text{in}}))$$

**Différence cruciale** : dans pre-norm, le **flux résiduel** ($x_{\text{in}}$ qui s'ajoute à la sortie) n'est **jamais normalisé**. Il transporte l'information à travers toutes les couches sans déformation. C'est ce qui permet d'entraîner stable des réseaux à 80, 120, 200 couches.

```
Post-norm (original, instable en profondeur) :
  x_in ─┬─→ sublayer ─→ + ─→ LayerNorm ─→ x_out
        │                ↑
        └────────────────┘

Pre-norm (moderne, stable) :
  x_in ─┬─→ LayerNorm ─→ sublayer ─→ + ─→ x_out
        │                            ↑
        └────────────────────────────┘
```

Dans pre-norm : une LayerNorm par sous-bloc (donc 2 par couche [[transformer-architecture|Transformer]] — une avant l'[[attention-mechanism|attention]], une avant le MLP), plus une **LayerNorm finale** avant l'un-embedding.

### Comment la normalisation absorbe le rôle du biais

C'est la connexion clé avec [[bias|le biais]].

Sans normalisation, dans une couche $y = Wx + b$ :
- $W$ apprend **comment** combiner les entrées.
- $b$ apprend **à quel niveau de base** placer la sortie.

Avec LayerNorm juste après (post-norm) ou juste avant la couche suivante (pre-norm), le re-centrage annule la moyenne des activations. Mathématiquement :

$$\text{si } z = Wx + b, \text{ alors } z - \text{mean}(z) = Wx - \text{mean}(Wx)$$

**Le $b$ s'annule.** Garder le biais dans la couche linéaire est donc strictement redondant avec LayerNorm. C'est pourquoi Llama (RMSNorm pre-norm partout) **n'a aucun biais** dans ses couches linéaires : ils n'apporteraient rien.

Pour RMSNorm (pas de re-centrage), un biais ne serait pas strictement annulé, mais le $\gamma$ joue déjà le rôle de scaling par dimension, et la pratique montre qu'on peut s'en passer sans dégradation.

### Comparaison BatchNorm / LayerNorm / RMSNorm

|                   | **BatchNorm**                                                       | **LayerNorm**                       | **RMSNorm**             |
|-------------------|---------------------------------------------------------------------|-------------------------------------|-------------------------|
| Normalise sur     | dimension batch (à feature fixe)                                    | dimension feature (à token fixe)    | idem LayerNorm          |
| Calcule mean+var  | oui (sur le batch)                                                  | oui (sur les features)              | non (juste la RMS)      |
| Affine            | $\gamma, \beta$                                                     | $\gamma, \beta$                     | $\gamma$ seulement      |
| Convient à        | CNN, batchs gros et fixes                                           | séquences, batchs variables         | Transformers modernes   |
| Inconvénient      | dépend du batch — problématique en inférence single-sample          | aucun majeur                        | aucun majeur            |

BatchNorm (2015) a débloqué les CNN profonds. LayerNorm (2016) en est la variante pour les modèles séquentiels (RNN puis Transformers). RMSNorm (2019) est la simplification minimaliste qui domine en 2024+.

## Examples & analogies

- **Pacemaker du réseau.** Sans normalisation, le signal cardiaque (les activations) part en arythmie au fil de la profondeur — soit tachycardie (explosion), soit bradycardie (extinction). La normalisation impose un rythme contrôlé à chaque couche, permettant à l'information de circuler sans s'emballer ni s'éteindre.
- **Système d'unités imposé.** Imagine un atelier où chaque ouvrier mesure dans une unité différente selon son humeur : centimètres, pouces, kilomètres. Ingérable. La normalisation, c'est imposer le système international à toutes les étapes — les ouvriers peuvent toujours faire ce qu'ils veulent (via $\gamma, \beta$), mais sur une base commune.

## Open questions

- Le mécanisme exact par lequel la normalisation lisse la loss landscape reste mal compris théoriquement.
- Existe-t-il des architectures profondes performantes **sans aucune** normalisation ? (cf. ReZero, SkipInit — résultats mitigés)
- Pourquoi le re-centrage de LayerNorm peut-il être supprimé sans perte par RMSNorm, alors qu'intuitivement il devrait apporter de la stabilité ?

## Related notes

- [[bias]]
- [[transformer-architecture]]
- [[attention-mechanism]]
- [[how-neural-networks-train]]
