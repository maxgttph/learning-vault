---
title: "Neural Networks — Overview"
aliases: ["neural networks", "neural nets"]
tags: [machine-learning, neural-networks, fundamentals]
created: 2026-05-26
source: conversation with Claude
lang: en
translations:
  - "[[neural-networks-overview.fr]]"
related: ["[[how-neural-networks-train.en]]", "[[transformer-architecture.en]]", "[[recurrent-networks-and-lstm.en]]", "[[reinforcement-learning.en]]", "[[world-models.en]]"]
---

# Neural Networks — Overview

## TL;DR

> A neural network is a parameterised function $f(x;\, \theta)$ that turns numbers into numbers through a cascade of matrix multiplications and non-linearities. Its billions of parameters $\theta$ are not hand-coded but learned by gradient descent on data.

## Key concepts

- **Neural network** — a parameterised function that can approximate any input-output relation, given enough parameters and data.
- **Weights / parameters** — the adjustable numbers, from millions to billions depending on the model. Llama 3 70B has ~70 billion.
- **Inference vs training** — using the model (forward pass only) vs learning its weights (forward + backward + update).
- **Universal approximation theorem** — a sufficiently large NN can approximate any continuous function. Theoretical: it guarantees neither the amount of data needed nor the practicality of training.

## Deep dive

### Beyond text

NNs are not reserved for LLMs. Any domain where an input maps to an output through a learnable relation qualifies:

- **Images** — Stable Diffusion, Midjourney, DALL-E (generation); classification, detection, segmentation.
- **Audio** — Whisper (transcription), Suno, MusicLM (generation).
- **Video** — Sora, Veo, Runway.
- **Science** — AlphaFold (3D protein structure), GraphCast (weather), AlphaGo.
- **Practical** — self-driving cars, fraud detection, materials simulation.

Rule of thumb: as soon as you can express "input → output" in numbers, an NN can try to learn the relation.

### The format of a trained model

A trained model is **not** one giant matrix, but:

- An **architecture** (a graph of operations, in code).
- A **weights file** (`.safetensors`, `.gguf`) containing hundreds of matrices and vectors.

For Llama 3 70B: ~70 billion parameters spread over hundreds of tensors, typically in FP16, sometimes quantised to 8/4/3 bits. See [[llm-inference-optimization.en]].

### Why it works now

Three combined factors, none sufficient on its own:

1. **Compute** — GPUs make massive matrix multiplications practical.
2. **Data** — the internet provides a near-unlimited corpus.
3. **Architecture** — the [[transformer-architecture.en|Transformer]] (2017) replaced RNNs, unlocking parallelisable training and the capture of long-range dependencies.

Add the **scaling laws** (Kaplan 2020, Chinchilla 2022): performance grows predictably with compute × data × parameters. That is what justified the massive investment.

### Limits and alternatives

NNs are bad at:

- Strict symbolic reasoning (formal mathematical proofs).
- Correctness guarantees (medical certification, safety-critical control).
- Radical out-of-distribution extrapolation.
- Small structured tabular data.

Depending on the case, other methods do better:

- **Gradient boosting** (XGBoost, LightGBM, CatBoost) — dominates on tabular data.
- **Linear/logistic regression** — interpretable, robust, ~linear relations.
- **Decision trees / random forests** — interpretable, small data.
- **SVMs** — the king of pre-2012 ML, still useful.
- **Bayesian methods** — when you want to model uncertainty explicitly.
- **Symbolic AI / expert systems** — hand-written rules, guarantees.
- **Neuro-symbolic** (e.g. AlphaGeometry) — a hybrid of NN + formal reasoning.

## Examples & analogies

- **A giant mixing desk.** Picture a desk with billions of faders. At the start the positions are random and the sound is horrible. For each reference song, you nudge every fader very slightly in the direction that brings the sound closer to the target. After millions of attempts, the desk reproduces all the music it was taught. Gradient descent is the algorithm that says which way to turn every fader simultaneously.

## Open questions

- Why does stochastic gradient descent find minima that generalise well? Not really understood theoretically.
- How can the appearance of "emergent capabilities" tied to scale be quantified precisely?
- Which domains currently dominated by NNs would benefit from a partial return to symbolic methods?

## Related notes

- [[how-neural-networks-train.en]]
- [[transformer-architecture.en]]
- [[recurrent-networks-and-lstm.en]]
- [[reinforcement-learning.en]]
- [[world-models.en]]
