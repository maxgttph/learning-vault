---
title: "Comment s'entraîne un réseau de neurones"
aliases: ["entraînement", "training", "backpropagation"]
tags: [machine-learning, training, gradient-descent, backpropagation]
created: 2026-05-26
source: conversation with Claude
related: ["[[neural-networks-overview]]", "[[transformer-architecture]]", "[[attention-mechanism]]", "[[bias]]", "[[normalization]]"]
---

# Comment s'entraîne un réseau de neurones

## TL;DR

> L'entraînement est une boucle : **forward pass** produit une prédiction, une **fonction de perte** mesure l'erreur, la **rétropropagation** calcule le gradient pour chaque paramètre, et un **optimizer** met à jour les poids dans la direction qui réduit la perte. Répété sur des milliards de batches.

## Key concepts

- **Forward pass** — appliquer le réseau à une entrée pour obtenir une prédiction.
- **Loss** — un scalaire mesurant l'erreur entre prédiction et cible (cross-entropy pour la classification, MSE pour la régression).
- **Backpropagation** — appliquer mécaniquement la règle de la chaîne en remontant le graphe de calcul, pour obtenir le gradient de la loss par rapport à chaque paramètre.
- **Gradient descent** — $w \leftarrow w - \eta \cdot \dfrac{\partial L}{\partial w}$. Cœur de l'apprentissage.
- **Optimizer** — SGD (simple), Adam / AdamW (ajoutent momentum et adaptation par paramètre — standard moderne).

## Deep dive

### Le paysage de la perte

Visualiser un espace à des milliards de dimensions (une par paramètre). À chaque point correspond une valeur de loss. L'entraînement = descendre ce paysage en suivant la pente locale (le **gradient**), pas à pas, jusqu'à trouver une vallée.

### Les six étapes du training loop

1. **Initialisation** — poids tirés aléatoirement (Xavier, He). Le modèle ne sait rien, sort du bruit.
2. **Forward pass** sur un batch d'exemples → prédictions.
3. **Loss** — comparer aux cibles. Pour un LLM, la cible est simplement le mot suivant dans le texte brut (**next-token prediction**), donc zéro annotation humaine nécessaire.
4. **Backpropagation** — la règle de la chaîne, appliquée en cascade de la sortie vers l'entrée, donne le gradient pour chaque paramètre du modèle. Les frameworks (PyTorch, JAX) calculent ça automatiquement via **autograd**.
5. **Update** — l'optimizer applique le pas. Avec SGD : $w \leftarrow w - \eta \cdot \dfrac{\partial L}{\partial w}$. Avec Adam : idem + momentum + learning rate adaptatif par paramètre.
6. **Répéter** — sur des milliards de batches. Llama 3 : 15 000 milliards de tokens, des semaines sur 16 000 GPU H100.

### Les trois phases d'un LLM moderne

1. **Pre-training** (90 % du compute) — next-token prediction sur du texte brut massif (Wikipédia, GitHub, livres). Sortie : un modèle qui "sait des choses" mais ne dialogue pas.
2. **SFT (Supervised Fine-Tuning)** — entraînement supplémentaire sur des paires `(instruction, bonne réponse)` curées par des humains. C'est ici que le modèle apprend le format "assistant".
3. **RLHF / DPO** — alignement aux préférences humaines. Des humains (ou un autre modèle juge) classent les réponses, et on ajuste les poids pour favoriser les préférées. C'est ce qui rend le modèle "utile, inoffensif, honnête".

### Exemple chiffré : un réseau à 2 couches

Architecture : 2 entrées → 2 neurones cachés (ReLU) → 1 sortie (sigmoid).

**État initial :** $W_1 = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}$, $W_2 = [0.5,\ 0.6]$, biais $= 0$.
Exemple : $x = [1,\ 2]$, cible $y = 1$.

**Forward :**
- $z_1 = [0.5,\ 1.1]$ → $h = \text{ReLU}(z_1) = [0.5,\ 1.1]$
- $z_2 = 0.5 \cdot 0.5 + 0.6 \cdot 1.1 = 0.91$ → $y_{\text{pred}} = \text{sigmoid}(0.91) \approx 0.713$
- **Loss (BCE)** $= -\ln(0.713) \approx 0.338$

**Backward** (astuce : pour sigmoid + BCE, $\dfrac{\partial L}{\partial z_2} = y_{\text{pred}} - y = -0.287$ se simplifie magnifiquement) :
- Propage en remontant via la règle de la chaîne.
- Donne un gradient pour chaque poids individuel.

**Update** avec $\eta = 0.1$ : tous les poids bougent légèrement dans la direction qui réduit la loss.

**Vérification :** refait le forward avec les nouveaux poids → $y_{\text{pred}} \approx 0.748$, loss $\approx 0.290$. La prédiction s'est rapprochée de la cible 1. Multiplié à 70 milliards de paramètres et $10^{13}$ tokens, c'est exactement cette mécanique qui produit un LLM moderne.

## Examples & analogies

- **Pupitre de mixage.** À chaque chanson cible, on tourne tous les potards d'un cran microscopique dans la direction qui rapproche le son du cible. La backpropagation est l'astuce mathématique qui dit, **simultanément**, dans quelle direction tourner chaque potard — sans avoir à les tester un par un.
- **Sigmoid + BCE.** Le couple est utilisé partout parce que leur dérivée combinée se simplifie en $y_{\text{pred}} - y_{\text{true}}$. Pas de hasard : c'est précisément cette élégance qui les rend pratiques.

## Open questions

- Pourquoi SGD trouve-t-il des minima "larges" qui généralisent, plutôt que des minima étroits qui surapprennent ? Compris empiriquement, mal théoriquement.
- Quand faut-il préférer DPO (plus simple) à RLHF (plus puissant) ?
- Comment réduire le coût énergétique de l'entraînement à grande échelle ? (~100M$ pour GPT-4)

## Related notes

- [[neural-networks-overview]]
- [[transformer-architecture]]
- [[attention-mechanism]]
- [[bias]]
- [[normalization]]
