---
title: "Inférence des LLM : KV cache et TurboQuant"
aliases: ["KV cache", "TurboQuant", "LLM inference"]
tags: [llm, inference, optimization, kv-cache, quantization]
created: 2026-05-26
source: conversation with Claude (TurboQuant ICLR 2026, PolarQuant AISTATS 2026)
related: ["[[attention-mechanism]]", "[[transformer-architecture]]", "[[recurrent-networks-and-lstm]]"]
---

# Inférence des LLM : KV cache et TurboQuant

## TL;DR

> L'inférence d'un LLM est dominée par la **mémoire**, pas le compute. Le **KV cache** évite de recalculer les vecteurs K et V des tokens passés à chaque génération, ramenant le coût par token de $O(n^2)$ à $O(n)$. **TurboQuant** (Google, ICLR 2026) compresse ce cache 6× sans perte mesurable et sans réentraînement.

## Key concepts

- **KV cache** — stockage des vecteurs Key et Value calculés pour les tokens passés, réutilisé à chaque nouveau token généré.
- **Quantization** — réduire la précision numérique (16 → 8 → 4 → 3 bits) pour économiser mémoire et compute.
- **GQA (Grouped Query Attention)** — plusieurs têtes Q partagent les mêmes K/V → réduit la taille du cache. Utilisée par Llama 3.
- **MQA (Multi-Query Attention)** — version extrême : une seule K/V pour toutes les têtes Q.
- **TurboQuant** — algorithme de Google compressant le KV cache à 3 bits/élément (réduction 6×) sans perte, jusqu'à 8× speedup attention sur H100.
- **PagedAttention** — gestion mémoire du cache façon pages OS (popularisée par vLLM).

## Deep dive

### Le problème

Pour générer le token $N+1$, [[attention-mechanism|l'attention]] doit regarder tous les tokens $1 \ldots N$. Naïvement, il faut recalculer K et V pour chacun de ces tokens à chaque pas de génération → coût $O(n^2)$ par token, donc $O(n^3)$ pour générer $n$ tokens. Catastrophique sur de longs contextes.

### La solution : KV cache

Observation clé : grâce au **masque causal**, les K et V des tokens passés **ne dépendent pas du nouveau token** — un token passé ne change pas avec ce qui vient après. On peut donc les mémoriser :

- Pour le token $N+1$ : on calcule seulement son $Q$, $K$, $V$ (et on ajoute K, V au cache).
- L'attention du nouveau Q se fait contre **tout le cache existant**.
- Coût par token : $O(n)$ au lieu de $O(n^2)$.

Pour générer 1000 tokens avec un contexte de 10 000, c'est la différence entre des secondes et des heures.

### Le nouveau goulot d'étranglement : la mémoire

Le KV cache grandit avec le contexte. Pour Llama 3 70B sur un contexte de 8000 tokens : **plusieurs Go de VRAM** par requête, en plus du modèle lui-même.

C'est pour ça que les LLM "explosent en mémoire" sur les longs contextes : pas tellement à cause du modèle (taille constante), mais à cause du cache qui grossit linéairement avec le contexte ET avec le nombre de requêtes simultanées.

D'où une floraison d'optimisations :

- **GQA (Grouped Query Attention)** — plusieurs têtes Q partagent leurs K/V. Llama 3 l'utilise.
- **MQA (Multi-Query Attention)** — version extrême : une seule K/V pour toutes les têtes.
- **Sliding window** — ne garder que les N derniers tokens dans le cache (Mistral).
- **PagedAttention** — gestion mémoire en pages, comme un OS, pour réutiliser efficacement (vLLM).

### TurboQuant (Google, ICLR 2026)

Compresse le KV cache à **3 bits par élément** (vs 16 bits en FP16) → réduction **6×**, sans perte mesurable, **sans réentraînement** ni données de calibration. Pur post-traitement à l'inférence.

**Comment ça marche** (deux étapes) :

1. **PolarQuant** (AISTATS 2026, première brique) — applique une **rotation orthogonale aléatoire** aux vecteurs K et V avant quantification. Pourquoi : les quantifieurs scalaires "MSE-optimaux" classiques introduisent un biais systématique dans l'estimation du produit scalaire $Q \cdot K^\top$ — or c'est exactement ce dont l'attention a besoin. La rotation aléatoire répartit l'erreur dans toutes les directions, éliminant le biais.

2. **Correction QJL 1-bit** — quantifie le résidu (erreur restante) sur 1 bit supplémentaire, rapprochant encore plus de la valeur d'origine.

**Impact pratique :**

- À 3.5 bits : qualité identique à FP16 (Needle score 0.997).
- Jusqu'à **8× speedup** de l'attention sur H100 (l'attention est *memory-bound* — moins de mémoire lue = plus rapide).
- Permet : contextes 6× plus longs à mémoire constante, OU 6× plus d'utilisateurs en parallèle sur le même matériel, OU faire tourner des gros modèles sur du matériel modeste.

C'est typique des progrès "systémiques" qui s'accumulent en parallèle des progrès "architecturaux" : les modèles ne se contentent pas de grossir, on apprend aussi à les exécuter de plus en plus efficacement.

## Examples & analogies

- **Cache = résumé des chapitres lus.** Sans KV cache, c'est comme relire tous les chapitres d'un livre à chaque nouveau paragraphe écrit. Avec, on garde le résumé synthétique des chapitres précédents sous la main, et on n'a qu'à intégrer le nouveau paragraphe.
- **Rotation aléatoire de PolarQuant.** Comparable à brouiller systématiquement l'orientation d'une grille avant d'y arrondir des points : l'erreur d'arrondi se répartit dans toutes les directions au lieu de s'aligner avec la grille, ce qui détruit les biais directionnels.

## Open questions

- Quels gains restent à extraire dans la compression KV cache ? Sub-2-bits envisageable ?
- Comment combiner TurboQuant avec PagedAttention en production (vLLM, TGI) ?
- À quel moment la compression du modèle (poids) devient-elle le nouveau goulot, par rapport au cache ?

## Related notes

- [[attention-mechanism]]
- [[transformer-architecture]]
- [[recurrent-networks-and-lstm]]
