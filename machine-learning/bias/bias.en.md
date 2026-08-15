---
title: "Bias in Linear Layers"
aliases: ["bias", "bias term"]
tags: [machine-learning, neural-networks, fundamentals]
created: 2026-05-29
source: conversation with Claude
lang: en
translations:
  - "[[bias.fr]]"
related: ["[[normalization.en]]", "[[how-neural-networks-train.en]]", "[[transformer-architecture.en]]", "[[attention-mechanism.en]]", "[[distillation.en]]"]
---

# Bias in Linear Layers

## TL;DR

> The bias $b$ in $y = Wx + b$ is a learned additive term that **shifts** the output of a linear layer. Without it, the hyperplane defined by $W$ is forced to pass through the origin, which severely restricts the network's expressive capacity. Modern Transformers (Llama, Mistral) often drop it because upstream normalisation absorbs its role.

## Key concepts

- **Bias ($b$)** — a learned vector (one scalar per output neuron) added after the linear combination $Wx$.
- **Geometric role** — it moves the hyperplane / activation threshold. Without a bias, every separating hyperplane passes through the origin.
- **Initialisation** — usually zero. Not a problem (unlike weights), because random weights already break the symmetry.
- **Training** — exactly like the weights, by backpropagation: $\partial L / \partial b = \partial L / \partial z$.
- **Removal in modern Transformers** — Llama omits biases because [[normalization.en|RMSNorm with pre-norm]] makes them redundant.

## Deep dive

### Mathematical form and geometric interpretation

In a linear layer:

$$y = Wx + b$$

where $W$ is the weight matrix and $b$ a bias vector (both learned). In 1D:

$$y = w \cdot x + b$$

This is the equation of a line. $w$ is the slope, $b$ is the **intercept** — where the line crosses the vertical axis. Without $b$, the line is forced through $(0, 0)$.

In $N$ dimensions, $Wx$ defines a **hyperplane through the origin**. The $+b$ term lets that hyperplane **translate** anywhere in the space.

### Why it is necessary (without normalisation)

Without a bias, $x = 0 \implies y = 0$ always. The neuron cannot emit a signal when all of its inputs are zero. More generally, the network is constrained so that all of its affine transformations pass through the origin — a structural constraint you get nothing for.

**A concrete example with ReLU**:

- Without bias: $\text{ReLU}(w \cdot x)$ — fires when $wx > 0$, i.e. around $x = 0$.
- With bias: $\text{ReLU}(wx + b)$ — fires when $wx + b > 0$, i.e. around $x = -b/w$.

The bias moves the neuron's **activation threshold**. That is what lets each neuron specialise on a specific region of input space (for instance, "detect when the value exceeds 25" rather than "detect when the value exceeds 0").

### Initialisation and training

**Initialisation**: zero, typically.

Why this does not cause the same problem as it does for weights:
- **Weights** initialised to zero create a pathological symmetry: every neuron in a layer computes the same thing, their gradients are identical → they never differentiate. Hence random initialisation (Xavier, He).
- **Biases** initialised to zero do not create that symmetry, because the random weights already break it. A bias can therefore start at zero without blocking learning.

Specialised variants:
- Bias of a classifier's last layer: initialise to $\log(\text{freq}_{\text{class}})$ to start close to a good estimate.
- Bias of the forget gates in LSTMs: initialise to 1 rather than 0 (an empirical trick).

**Training**: exactly like the weights. If $z = Wx + b$ and you know $\partial L / \partial z$, then:

$$\frac{\partial L}{\partial b} = \frac{\partial L}{\partial z} \qquad \frac{\partial L}{\partial W} = \frac{\partial L}{\partial z} \cdot x^\top$$

Standard update:

$$b \leftarrow b - \eta \cdot \frac{\partial L}{\partial b}$$

Adam applies in the same way (momentum + per-parameter adaptation). The bias is a parameter like any other — no special treatment in optimiser code.

### Dropping the bias in modern Transformers

Recent models (Llama, Mistral) have **no biases at all** in their linear layers — neither in the MLP nor in the Q/K/V/O projections of [[attention-mechanism.en|attention]]. Why they can afford this:

**1. Normalisation absorbs the role.** With [[normalization.en|LayerNorm/RMSNorm]] before each sub-block (pre-norm), the activations are already re-centred before the next linear layer. A bias afterwards would be redundant.

A demonstration for LayerNorm: if $z = Wx + b$, then $\text{mean}(z) = \text{mean}(Wx) + b$. Subtracting the mean gives $z - \text{mean}(z) = Wx - \text{mean}(Wx)$ — **the $b$ cancels mathematically**. It is a ghost parameter.

**2. Parameter savings.** Out of 70 billion parameters, removing biases saves a few hundred million. Marginal in size, but it simplifies the architecture.

**3. Empirical.** Controlled studies: identical (or slightly better) performance without biases, provided normalisation is clean.

### The current landscape

| Architecture | Bias |
|---|---|
| Standalone MLP, small network | Present everywhere — necessary |
| GPT-2, BERT (post-norm + full LayerNorm) | Present everywhere — historical convention |
| Llama, Mistral (pre-norm + RMSNorm) | **Removed** from linear layers |

Even when the bias is removed, RMSNorm's $\gamma$ (a learned parameter) already provides per-dimension scaling, which is enough flexibility in practice.

## Examples & analogies

- **A specialised thermostat.** A neuron detecting "it's hot" would like to fire around 25 °C. Without a bias, its threshold is stuck at 0 °C. The bias lets it put its personal threshold wherever it wants.
- **The intercept.** From school geometry: $y = ax + b$. $a$ is the slope, $b$ is where the line crosses the vertical axis. Without $b$, every line passes through the origin — severely restrictive.

## Open questions

- Why does initialising LSTM forget-gate biases to 1 (rather than 0) help so much?
- Are there architectures where reintroducing biases would be useful despite normalisation?
- Does RMSNorm's $\gamma$ really compensate for the absence of a bias in all conditions, or are there pathological cases?

## Related notes

- [[normalization.en]]
- [[how-neural-networks-train.en]]
- [[transformer-architecture.en]]
- [[attention-mechanism.en]]
- [[distillation.en]]
