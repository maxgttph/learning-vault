---
title: "Neural Networks — Vue d'ensemble"
aliases: ["réseaux de neurones", "neural networks"]
tags: [machine-learning, neural-networks, fundamentals]
created: 2026-05-26
source: conversation with Claude
related: ["[[how-neural-networks-train]]", "[[transformer-architecture]]", "[[recurrent-networks-and-lstm]]", "[[reinforcement-learning]]", "[[world-models]]"]
---

# Neural Networks — Vue d'ensemble

## TL;DR

> Un réseau de neurones est une fonction paramétrée $f(x;\, \theta)$ qui transforme des nombres en nombres via une cascade de multiplications matricielles et de non-linéarités. Ses milliards de paramètres $\theta$ ne sont pas codés à la main mais appris par descente de gradient sur des données.

## Key concepts

- **Réseau de neurones** — fonction paramétrée qui peut approximer n'importe quelle relation entrée-sortie, à condition d'avoir assez de paramètres et de données.
- **Poids / paramètres** — les nombres ajustables, des millions à des milliards selon le modèle. Llama 3 70B en a ~70 milliards.
- **Inférence vs entraînement** — utiliser le modèle (forward pass seul) vs apprendre ses poids (forward + backward + update).
- **Théorème d'approximation universelle** — un NN suffisamment grand peut approximer n'importe quelle fonction continue. Théorique : ne garantit ni la quantité de données, ni la praticabilité de l'entraînement.

## Deep dive

### Au-delà du texte

Les NN ne sont pas réservés aux LLM. Tout domaine où une entrée se traduit en sortie via une relation apprenable est concerné :

- **Images** — Stable Diffusion, Midjourney, DALL-E (génération) ; classification, détection, segmentation.
- **Audio** — Whisper (transcription), Suno, MusicLM (génération).
- **Vidéo** — Sora, Veo, Runway.
- **Sciences** — AlphaFold (structure 3D des protéines), GraphCast (météo), AlphaGo.
- **Pratique** — voitures autonomes, détection de fraude, simulation de matériaux.

Règle d'or : dès qu'on peut exprimer « entrée → sortie » en nombres, un NN peut tenter d'apprendre la relation.

### Format d'un modèle entraîné

Un modèle entraîné, ce n'est **pas** une seule matrice géante, mais :

- Une **architecture** (graphe d'opérations en code).
- Un **fichier de poids** (`.safetensors`, `.gguf`) contenant des centaines de matrices et vecteurs.

Pour Llama 3 70B : ~70 milliards de paramètres répartis sur des centaines de tenseurs, typiquement en FP16, parfois quantifiés à 8/4/3 bits. Voir [[llm-inference-optimization]].

### Pourquoi ça marche maintenant

Trois facteurs combinés, aucun ne suffit seul :

1. **Compute** — les GPU rendent praticables les multiplications matricielles massives.
2. **Données** — Internet fournit un corpus quasi-illimité.
3. **Architecture** — le [[transformer-architecture|Transformer]] (2017) a remplacé les RNN, débloquant l'entraînement parallélisable et la capture de dépendances longues.

S'ajoutent les **scaling laws** (Kaplan 2020, Chinchilla 2022) : performance croît de façon prévisible avec compute × données × paramètres. Ça a justifié les investissements massifs.

### Limites et alternatives

Les NN sont mauvais sur :

- Le raisonnement symbolique strict (preuves mathématiques formelles).
- Les garanties de correction (certification médicale, contrôle critique).
- L'extrapolation radicale hors distribution.
- Les petites données structurées tabulaires.

Selon le cas, d'autres méthodes sont meilleures :

- **Gradient boosting** (XGBoost, LightGBM, CatBoost) — domine sur données tabulaires.
- **Régressions linéaires/logistiques** — interprétables, robustes, relations ~linéaires.
- **Arbres de décision / forêts aléatoires** — interprétables, petites données.
- **SVM** — roi du ML pré-2012, encore utile.
- **Méthodes bayésiennes** — quand on veut modéliser explicitement l'incertitude.
- **IA symbolique / systèmes experts** — règles écrites à la main, garanties.
- **Neuro-symbolique** (ex. AlphaGeometry) — hybride NN + raisonnement formel.

## Examples & analogies

- **Pupitre de mixage géant.** Imagine un pupitre à des milliards de potentiomètres. Au début, les positions sont aléatoires, le son est horrible. Pour chaque chanson de référence, on ajuste très légèrement chaque potard dans la direction qui rapproche le son du cible. Au bout de millions d'essais, le pupitre reproduit toute la musique enseignée. La descente de gradient, c'est l'algorithme qui dit dans quelle direction tourner chaque potard simultanément.

## Open questions

- Pourquoi la descente de gradient stochastique trouve-t-elle des minima qui généralisent bien ? Pas vraiment compris théoriquement.
- Comment quantifier précisément l'apparition de "capacités émergentes" liées à l'échelle ?
- Quels domaines actuellement dominés par les NN bénéficieraient d'un retour à du symbolique partiel ?

## Related notes

- [[how-neural-networks-train]]
- [[transformer-architecture]]
- [[recurrent-networks-and-lstm]]
- [[reinforcement-learning]]
- [[world-models]]
