---
title: "How a Neural Network Trains"
aliases: ["training", "backpropagation"]
tags: [machine-learning, training, gradient-descent, backpropagation]
created: 2026-05-26
source: conversation with Claude
lang: en
translations:
  - "[[how-neural-networks-train.fr]]"
related: ["[[neural-networks-overview.en]]", "[[transformer-architecture.en]]", "[[attention-mechanism.en]]", "[[bias.en]]", "[[normalization.en]]", "[[recurrent-networks-and-lstm.en]]", "[[reinforcement-learning.en]]", "[[world-models.en]]", "[[distillation.en]]", "[[mixture-of-experts.en]]"]
---

# How a Neural Network Trains

## TL;DR

> Training is a loop: the **forward pass** produces a prediction, a **loss function** measures the error, **backpropagation** computes the gradient for every parameter, and an **optimiser** updates the weights in the direction that reduces the loss. Repeated over billions of batches.

## Key concepts

- **Forward pass** — applying the network to an input to obtain a prediction.
- **Loss** — a scalar measuring the error between prediction and target (cross-entropy for classification, MSE for regression).
- **Backpropagation** — mechanically applying the chain rule backwards through the computation graph, to obtain the gradient of the loss with respect to every parameter.
- **Gradient descent** — $w \leftarrow w - \eta \cdot \dfrac{\partial L}{\partial w}$. The heart of learning.
- **Optimiser** — SGD (simple), Adam / AdamW (adding momentum and per-parameter adaptation — the modern standard).

## Deep dive

### The loss landscape

Picture a space with billions of dimensions (one per parameter). Every point corresponds to a loss value. Training = descending that landscape by following the local slope (the **gradient**), step by step, until you find a valley.

### The six steps of the training loop

1. **Initialisation** — weights drawn randomly (Xavier, He). The model knows nothing and outputs noise.
2. **Forward pass** over a batch of examples → predictions.
3. **Loss** — compare with the targets. For an LLM, the target is simply the next word in the raw text (**next-token prediction**), so zero human annotation is needed.
4. **Backpropagation** — the chain rule, applied in cascade from output back to input, gives the gradient for every model parameter. Frameworks (PyTorch, JAX) compute this automatically via **autograd**.
5. **Update** — the optimiser applies the step. With SGD: $w \leftarrow w - \eta \cdot \dfrac{\partial L}{\partial w}$. With Adam: the same + momentum + a per-parameter adaptive learning rate.
6. **Repeat** — over billions of batches. Llama 3: 15 trillion tokens, weeks on 16,000 H100 GPUs.

### The three phases of a modern LLM

1. **Pre-training** (90% of the compute) — next-token prediction on massive raw text (Wikipedia, GitHub, books). Output: a model that "knows things" but does not converse.
2. **SFT (Supervised Fine-Tuning)** — extra training on `(instruction, good answer)` pairs curated by humans. This is where the model learns the "assistant" format.
3. **RLHF / DPO** — alignment with human preferences. Humans (or another model acting as judge) rank the answers, and the weights are adjusted to favour the preferred ones. This is what makes the model "helpful, harmless, honest".

### A worked numerical example: a 2-layer network

Architecture: 2 inputs → 2 hidden neurons (ReLU) → 1 output (sigmoid).

**Initial state:** $W_1 = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}$, $W_2 = [0.5,\ 0.6]$, biases $= 0$.
Example: $x = [1,\ 2]$, target $y = 1$.

**Forward:**
- $z_1 = [0.5,\ 1.1]$ → $h = \text{ReLU}(z_1) = [0.5,\ 1.1]$
- $z_2 = 0.5 \cdot 0.5 + 0.6 \cdot 1.1 = 0.91$ → $y_{\text{pred}} = \text{sigmoid}(0.91) \approx 0.713$
- **Loss (BCE)** $= -\ln(0.713) \approx 0.338$

**Backward** (a nice trick: for sigmoid + BCE, $\dfrac{\partial L}{\partial z_2} = y_{\text{pred}} - y = -0.287$ simplifies beautifully):
- Propagate backwards via the chain rule.
- Yields a gradient for each individual weight.

**Update** with $\eta = 0.1$: every weight moves slightly in the direction that reduces the loss.

**Check:** redo the forward pass with the new weights → $y_{\text{pred}} \approx 0.748$, loss $\approx 0.290$. The prediction has moved closer to the target of 1. Scaled up to 70 billion parameters and $10^{13}$ tokens, this exact mechanism is what produces a modern LLM.

## Examples & analogies

- **The mixing desk.** For every target song, you turn all the faders by a microscopic notch in the direction that brings the sound closer to the target. Backpropagation is the mathematical trick that says, **simultaneously**, which way to turn every fader — without having to test them one by one.
- **Sigmoid + BCE.** The pair is used everywhere because their combined derivative simplifies to $y_{\text{pred}} - y_{\text{true}}$. That is no accident: that elegance is precisely what makes them practical.

## Open questions

- Why does SGD find "wide" minima that generalise, rather than narrow ones that overfit? Understood empirically, poorly understood theoretically.
- When should DPO (simpler) be preferred over RLHF (more powerful)?
- How can the energy cost of large-scale training be reduced? (~\$100M for GPT-4)

## Related notes

- [[neural-networks-overview.en]]
- [[transformer-architecture.en]]
- [[attention-mechanism.en]]
- [[bias.en]]
- [[normalization.en]]
- [[recurrent-networks-and-lstm.en]]
- [[reinforcement-learning.en]]
- [[world-models.en]]
- [[distillation.en]]
- [[mixture-of-experts.en]]
