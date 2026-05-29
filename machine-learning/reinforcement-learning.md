---
title: "Reinforcement Learning vs Supervised Learning"
aliases: ["RL", "apprentissage par renforcement", "reinforcement learning", "supervised learning", "apprentissage supervisé"]
tags: [machine-learning, reinforcement-learning, supervised-learning, learning-paradigms, fundamentals]
created: 2026-05-29
source: conversation with Claude
related: ["[[how-neural-networks-train]]", "[[neural-networks-overview]]", "[[transformer-architecture]]", "[[recurrent-networks-and-lstm]]"]
---

# Reinforcement Learning vs Supervised Learning

## TL;DR

> Le supervisé apprend d'**étiquettes** (« voici la bonne réponse »), le reinforcement learning apprend d'une **récompense scalaire** obtenue en agissant (« c'était bien / mal »). Ce sont des *paradigmes d'apprentissage* — ils définissent **d'où vient le signal de gradient**, pas la forme du réseau. Ils sont donc **orthogonaux** aux architectures comme le [[transformer-architecture|Transformer]] ou le [[recurrent-networks-and-lstm|RNN]] : on les combine, on ne les oppose pas.

## Key concepts

- **Supervised learning** — apprendre une fonction $f_\theta(x)\approx y$ à partir de paires (entrée, bonne réponse) connues. Signal dense et direct.
- **Reinforcement learning** — un **agent** interagit avec un **environnement** : il observe un état, choisit une action, reçoit une récompense. Aucune « bonne réponse » fournie.
- **Politique** $\pi_\theta(a\mid s)$ — la stratégie de l'agent : probabilité de choisir l'action $a$ dans l'état $s$.
- **Récompense escomptée** — l'objectif du RL : maximiser $\sum_t \gamma^t r_t$, la somme des récompenses futures pondérée par $\gamma\in[0,1)$.
- **Credit assignment** — le problème de savoir *quelle* action passée mérite le crédit (ou le blâme) d'une récompense qui n'arrive que bien plus tard.
- **Exploration vs exploitation** — tenter des actions inconnues pour découvrir mieux, vs exploiter ce qu'on sait déjà payant. Dilemme propre au RL.
- **Paradigme ≠ architecture** — le paradigme dit *d'où vient le signal*, l'architecture dit *quelle est la forme de* $f_\theta$. Axes indépendants.

## Deep dive

### Supervised learning : apprendre d'un corrigé

On dispose d'un jeu de données **étiqueté**, des paires (entrée, bonne réponse) :

$$\mathcal{D} = \{(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)\}$$

Le modèle apprend $f_\theta(x)\approx y$ en minimisant une **fonction de perte** qui mesure l'écart à la vérité connue :

$$\mathcal{L}(\theta) = \frac{1}{n}\sum_{i=1}^{n} \ell\big(f_\theta(x_i),\, y_i\big)$$

La caractéristique clé est que le **signal est dense et direct** : pour *chaque* exemple on connaît la bonne réponse, et le gradient indique précisément comment corriger. C'est de la reconnaissance de motifs — « voici l'image, voici que c'est un chat ». La mécanique du gradient elle-même (forward → loss → backprop → update) est détaillée dans [[how-neural-networks-train]] ; le supervisé en est le cas canonique.

### Reinforcement learning : apprendre en agissant

Il n'y a **pas de bonne réponse fournie**. Un **agent** interagit avec un **environnement** : il observe un état $s_t$, choisit une action $a_t$, et reçoit une **récompense** $r_t$ (un scalaire, souvent nul) plus un nouvel état. Le cadre formel est le *Markov Decision Process* :

- **État** $s_t$ — la situation courante.
- **Action** $a_t$ — ce que l'agent fait.
- **Récompense** $r_t$ — le feedback chiffré.
- **Politique** $\pi_\theta(a\mid s)$ — la stratégie, ce qu'on apprend.

L'objectif n'est pas de coller à une étiquette mais de **maximiser la récompense cumulée future** :

$$J(\theta) = \mathbb{E}_{\pi_\theta}\!\left[\sum_{t=0}^{\infty} \gamma^t\, r_t\right]$$

où $\gamma\in[0,1)$ est le **facteur d'escompte** qui arbitre entre présent et futur.

Trois difficultés sont propres au RL et absentes du supervisé :

- **Feedback rare et différé** — on peut jouer 200 coups aux échecs et n'apprendre qu'à la fin qu'on a perdu. Quel coup était mauvais ? C'est le *credit assignment problem*.
- **Pas de cible directe** — personne ne dit « le bon coup était X » ; on sait seulement si le résultat global était bon.
- **Exploration vs exploitation** — il faut parfois tenter des actions sous-optimales pour découvrir mieux, un arbitrage qui n'existe pas en supervisé (où les données sont fixées d'avance).

### Le contraste en une vue

| | Supervised | Reinforcement |
|---|---|---|
| Signal | étiquette « la vraie réponse » | récompense scalaire « bien / mal » |
| Densité | une cible par exemple | souvent rare, différée |
| Données | fixes, fournies d'avance | générées par l'agent en agissant |
| But | imiter | optimiser un comportement |

### Le point crucial : paradigme ≠ architecture

C'est la source habituelle de confusion. RL/supervisé et Transformer/RNN ne sont **pas sur le même axe**.

- **Transformer et RNN sont des *architectures*** : des façons de câbler un réseau, c'est-à-dire *la forme de la fonction* $f_\theta$. Ils répondent à « comment représenter et traiter l'entrée ? ».
- **Supervised et reinforcement learning sont des *paradigmes d'entraînement*** : des façons de calculer le signal qui ajuste $\theta$. Ils répondent à « d'où vient le gradient ? ».

Les deux axes se combinent librement :

```
                  Architecture (le "quoi")
                  Transformer / RNN / CNN / MLP
                          ▲
                          │
   Paradigme (le "comment on apprend")  ──┘
   Supervised / Reinforcement / Self-supervised
```

Exemples de combinaisons : Transformer + supervisé (un classifieur de texte fine-tuné sur des labels) ; RNN + RL (contrôleurs robotiques récurrents, premiers agents Atari à mémoire) ; Transformer + RL (le **RLHF** des LLM conversationnels).

### Le cas du LLM : les deux paradigmes empilés

Le LLM est l'illustration parfaite de la complémentarité — **même architecture** (un transformer) du début à la fin, seul le **paradigme** change d'une étape à l'autre :

1. **Pré-entraînement self-supervisé** — prédire le mot suivant sur du texte brut. Les étiquettes sont gratuites (c'est le texte lui-même), d'où « self-supervisé ».
2. **SFT (Supervised Fine-Tuning)** — sur des paires (instruction, bonne réponse) curées par des humains.
3. **RLHF (Reinforcement Learning from Human Feedback)** — un signal de récompense issu des préférences humaines affine le comportement là où il n'existe pas de réponse unique (ton, utilité, sécurité).

Le détail de ces trois phases côté *mécanique d'entraînement* vit dans [[how-neural-networks-train]] ; cette fiche-ci se concentre sur *pourquoi* ce sont des paradigmes distincts.

## Examples & analogies

- **Le corrigé vs le vélo.** Le supervisé, c'est apprendre avec le corrigé sous les yeux : à chaque exercice on voit la réponse exacte. Le RL, c'est apprendre à faire du vélo : personne ne donne « l'angle exact du guidon » — on tombe (récompense négative) ou on avance (positive), et on ajuste.
- **RLHF.** Quand un LLM conversationnel devient « utile, inoffensif, honnête », ce n'est pas en lui montrant la bonne réponse (il n'y en a pas une seule), mais en classant ses réponses par préférence et en renforçant les préférées — du RL appliqué à un transformer.

## Open questions

- Les familles d'algorithmes RL : *value-based* (Q-learning) vs *policy gradient* (REINFORCE, PPO) — candidate pour une fiche dédiée.
- Comment fonctionne précisément le RLHF (modèle de récompense, PPO, puis DPO comme alternative sans RL explicite) ?
- Le self-supervised mérite-t-il sa propre fiche, comme troisième paradigme à part entière ?
- Faut-il à terme scinder `machine-learning/` en sous-dossiers `architectures/` et `training/` ? (Pas avant que le cluster le justifie.)

## Related notes

- [[how-neural-networks-train]]
- [[neural-networks-overview]]
- [[transformer-architecture]]
- [[recurrent-networks-and-lstm]]
