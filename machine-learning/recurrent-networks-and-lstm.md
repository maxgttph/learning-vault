---
title: "Réseaux récurrents (RNN) et LSTM"
aliases: ["RNN", "LSTM", "GRU", "réseaux récurrents"]
tags: [machine-learning, rnn, lstm, sequences, gradient]
created: 2026-05-29
source: conversation with Claude
related: ["[[neural-networks-overview]]", "[[how-neural-networks-train]]", "[[transformer-architecture]]", "[[attention-mechanism]]", "[[embeddings-and-tokenization]]"]
---

# Réseaux récurrents (RNN) et LSTM

## TL;DR

> Un **RNN** traite une séquence élément par élément en transportant un état caché $h_t$ (une mémoire). Propager le gradient dans le temps revient à **multiplier** une longue chaîne de termes, ce qui le fait s'évanouir → mémoire courte. Le **LSTM** ajoute une cellule mémoire mise à jour par **addition** et contrôlée par des **portes apprises**, offrant au gradient une autoroute où il se conserve — ce qui rend les dépendances longues apprenables.

## Key concepts

- **Séquence** — suite ordonnée d'éléments où l'ordre compte (mots, séries temporelles, audio, frames vidéo).
- **État caché $h_t$** — vecteur résumant « tout ce qui a été lu jusqu'au pas $t$ » ; sortie brute du RNN à chaque pas.
- **Récurrence** — la même opération et le **même jeu de poids** réappliqués à chaque pas de temps, d'où le besoin de $h_{t-1}$ pour calculer $h_t$ (séquentiel par construction).
- **BPTT (backpropagation through time)** — rétropropagation sur le RNN « déplié » dans le temps ; le gradient remonte la chaîne des $h_t$.
- **Vanishing / exploding gradient** — produit d'une longue chaîne de dérivées qui tend vers 0 (mémoire perdue) ou explose (divergence).
- **Cellule mémoire $C_t$** (LSTM) — « tapis roulant » d'information modifié par petites touches plutôt que réécrit.
- **Portes** — sigmoïdes sortant des valeurs dans $[0,1]$ ; dosent ce qu'on oublie, écrit et lit.
- **GRU** — variante à 2 portes, plus légère que le LSTM, souvent aussi performante.

## Deep dive

### Ce qu'un RNN prend en entrée et comment il « traite pas à pas »

L'entrée est une **liste d'embeddings** : la phrase est tokenisée, chaque token devient un vecteur (voir [[embeddings-and-tokenization]]). Contrairement à un Transformer, le RNN ne voit pas tous ces vecteurs en même temps : il les **consomme un par un**, dans l'ordre, en mettant à jour son état caché.

$$h_t = \tanh(W_x\, x_t + W_h\, h_{t-1} + b)$$

Pour calculer $h_2$ il faut $h_1$ : il y a une dépendance, donc impossible de paralléliser sur la séquence. C'est la différence d'**architecture** (pas d'encodage) avec le Transformer : l'embedding est l'entrée commune aux deux ; c'est la boucle récurrente vs l'attention qui impose séquentiel vs parallèle. Les Transformers ont précisément été inventés pour échapper à cette lenteur séquentielle.

### L'output dépend de la tâche

Le RNN/LSTM produit un vecteur $h_t$ **à chaque pas**. C'est la sortie brute ; une **couche de sortie** ajoutée par-dessus la transforme selon l'usage :

- **Many-to-one** (sentiment, classif. de texte) : on ne garde que le dernier $h_T$ (résumé global) → une prédiction par phrase.
- **Many-to-many aligné** (étiquetage grammatical) : une prédiction sur chaque $h_t$ → un tag par mot.
- **Génération / next-token** : sur chaque $h_t$, un softmax sur le vocabulaire prédit le mot suivant, réinjecté au pas d'après.
- **Seq2seq** (traduction) : un RNN encode toute la source dans son état final, un second décode mot à mot.

### Pourquoi le RNN simple échoue : le gradient dans le temps

À l'entraînement on déroule la boucle : un RNN sur 50 mots devient, du point de vue du gradient, un calcul à **50 niveaux** réutilisant le même $W_h$ (ce ne sont pas 50 couches physiques distinctes — c'est **une** couche appliquée 50 fois ; la profondeur est *temporelle*, pas spatiale). Le backward remonte la chaîne des $h_t$ via la règle de la chaîne (voir [[how-neural-networks-train]]) :

$$\frac{\partial L}{\partial h_1} = \frac{\partial L}{\partial h_T} \cdot \frac{\partial h_T}{\partial h_{T-1}} \cdots \frac{\partial h_2}{\partial h_1}$$

Chaque facteur implique $W_h$ et $\tanh' \le 1$. Multiplier ~50 termes $\le 1$ fait fondre le produit vers 0 : le gradient **s'évanouit**, le début de séquence est oublié. Le RNN simple ne retient que ~10–20 pas. Sa mémoire est « short-term ». Le nom **Long Short-Term Memory** annonce le programme : une mémoire court terme **qui dure**.

> Le calcul des $h_t$ (forward pass) est identique à l'entraînement et à l'inférence. La différence : à l'inférence on s'arrête après le forward ; à l'entraînement on enchaîne le backward, qui circule **à travers les $h_t$**. Les $h_t$ ne sont donc pas « un truc d'inférence » — ils sont le support même du gradient.

### Le LSTM : séparer la mémoire de la sortie

Le LSTM fait circuler **deux** vecteurs dans le temps : la cellule mémoire $C_t$ (le tapis roulant, peu modifié) et l'état caché $h_t$ (ce qu'on expose, dérivé de $C_t$). Trois portes ($\sigma$ → valeurs dans $[0,1]$, multipliées composante par composante) :

- **Oubli** $f_t = \sigma(W_f\,[h_{t-1}, x_t] + b_f)$ — que garder de l'ancienne mémoire.
- **Entrée** $i_t = \sigma(W_i\,[h_{t-1}, x_t] + b_i)$ — que écrire de neuf.
- **Sortie** $o_t = \sigma(W_o\,[h_{t-1}, x_t] + b_o)$ — que lire de la mémoire.

Mise à jour — l'équation clé est une **addition**, pas une réécriture matricielle :

$$C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t, \qquad \tilde{C}_t = \tanh(W_C\,[h_{t-1}, x_t] + b_C)$$
$$h_t = o_t \odot \tanh(C_t)$$

```
mémoire  C₀ ──→ C₁ ──→ C₂ ──→ C₃     ← tapis roulant (addition contrôlée)
                 │       │       │
sortie          h₁      h₂      h₃     ← lecture filtrée par o_t
```

### Pourquoi ça résout le vanishing gradient

À cause de la forme additive, le gradient qui remonte la mémoire est essentiellement :

$$\frac{\partial C_t}{\partial C_{t-1}} \approx f_t$$

$f_t$ est **appris et contextuel**. Si l'info est importante, le réseau apprend $f_t \approx 1$ et alors $1 \times 1 \times \cdots \approx 1$ : le gradient traverse des centaines de pas sans s'évanouir (le **« constant error carousel »**). Quand il faut oublier, $f_t \approx 0$ coupe volontairement. On remplace une **multiplication subie** (qui détruit le gradient) par une **addition contrôlée par des portes apprises** (qui le préserve à la demande). C'est exactement l'idée des *skip connections* (ResNet) et du flux résiduel des [[transformer-architecture|Transformers]] : offrir au gradient un chemin additif.

Coût : ~4× les paramètres d'un RNN simple (3 portes + le candidat $\tilde{C}_t$), d'où le **GRU** qui fait presque pareil avec 2 portes.

### Taille de $h$ ≠ longueur de séquence

Deux dimensions indépendantes, souvent confondues :

- **Taille de $h$** (hidden size, ex. 128) — la *largeur* de la mémoire, un hyperparamètre fixe. Tous les $h_t$ ont cette taille, quelle que soit la phrase. (L'entrée $x_t$, ex. embedding de dim. 300, a sa propre taille, distincte.)
- **Longueur de séquence** — le *nombre d'itérations* de la boucle. Le RNN n'a **aucune limite dure** : il boucle autant de fois qu'il y a de mots.

La seule limite est **molle** : la mémoire du début s'estompe (vanishing gradient) — le LSTM la repousse sans l'éliminer. La limite **dure** de longueur, elle, est une caractéristique des **Transformers** (fenêtre de contexte fixe), pas des RNN — compromis fondamental entre les deux familles.

### Panorama des RNN

**Par type de cellule :**

| Cellule | Spécificité | Usage |
|---|---|---|
| RNN simple (Elman) | 1 couche récurrente $\tanh$ ; vanishing gradient | pédagogie ; rare en prod |
| Jordan | récurrence depuis la sortie, pas l'état caché | historique |
| **LSTM** | cellule mémoire + 3 portes | ★ très utilisé |
| **GRU** | 2 portes, plus léger | ★ très utilisé |
| Peephole LSTM | les portes voient $C_t$ | niche (timing précis) |

**Par topologie :**

| Architecture | Spécificité | Domaines |
|---|---|---|
| **Bidirectionnel** (Bi-LSTM/GRU) | lit la séquence dans les 2 sens | ★ NER, POS-tagging, reconnaissance vocale |
| Deep / Stacked | plusieurs couches récurrentes empilées (profondeur spatiale + temporelle) | tâches complexes |
| **Encoder-Decoder (Seq2Seq)** | un RNN encode, un autre décode | ★ traduction, résumé |
| Seq2Seq + Attention | le décodeur regarde tous les états encodeur | précurseur du Transformer (Bahdanau 2014) |
| Recursive NN (TreeRNN) | structure d'arbre | analyse syntaxique |
| Echo State / Reservoir | poids récurrents fixes aléatoires, seule la sortie est apprise | séries temporelles, recherche |
| Neural Turing Machine / DNC | RNN + mémoire externe adressable | raisonnement algorithmique |

**Les plus utilisés** : LSTM et GRU (workhorses), Bi-LSTM (longtemps standard NLP avant 2018), Seq2Seq + attention.

### Mise en perspective

Depuis 2017, les [[transformer-architecture|Transformers]] ont largement supplanté les RNN en NLP (parallélisables, meilleures dépendances longues). Les RNN restent pertinents pour le **streaming temps réel**, l'**embarqué/faible compute** et les **séries temporelles** modestes. Les modèles **état-espace** récents (Mamba, S4) réhabilitent l'idée récurrente avec de meilleures propriétés.

## Examples & analogies

- **Chaîne de montage avec tapis roulant central** ($C_t$) : à chaque poste, un ouvrier retire des choses du tapis (porte d'oubli), un autre en dépose (porte d'entrée), un troisième lit à voix haute pour le poste suivant sans forcément retirer (porte de sortie). Une pièce importante peut rester intacte sur des centaines de postes — là où un RNN simple reconstruit tout à chaque poste et perd l'original.
- **Accord à distance** : « le **chat** que j'ai vu hier dans le jardin… **dort** ». Capturer cet accord sujet-verbe sur de nombreux mots est précisément ce qu'un LSTM rend apprenable et un RNN simple non.

## Open questions

- Détail pas-à-pas de la BPTT tronquée (truncated BPTT) utilisée en pratique pour les longues séquences.
- GRU vs LSTM : quand l'un bat-il réellement l'autre empiriquement ?
- Comment les modèles état-espace (Mamba, S4) combinent-ils récurrence et parallélisation à l'entraînement ?

## Related notes

- [[neural-networks-overview]]
- [[how-neural-networks-train]]
- [[transformer-architecture]]
- [[attention-mechanism]]
- [[embeddings-and-tokenization]]
