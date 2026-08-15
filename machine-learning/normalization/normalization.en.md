---
title: "Normalization (LayerNorm, RMSNorm)"
aliases: ["LayerNorm", "RMSNorm", "normalization", "pre-norm"]
tags: [machine-learning, neural-networks, transformer, normalization]
created: 2026-05-29
source: conversation with Claude
lang: en
translations:
  - "[[normalization.fr]]"
related: ["[[bias.en]]", "[[transformer-architecture.en]]", "[[attention-mechanism.en]]", "[[how-neural-networks-train.en]]"]
---

# Normalization (LayerNorm, RMSNorm)

## TL;DR

> Normalization forces activations to stay within a controlled range at every layer, preventing the signal and gradients from exploding or vanishing in deep networks. Without it you cap out at ~15 layers; with it, 200+ layers are trainable. The modern standard is **RMSNorm + pre-norm**, simpler and more stable than the original (LayerNorm + post-norm).

## Key concepts

- **LayerNorm** — normalises a vector of activations to mean 0, variance 1, then applies a learned affine transform $\gamma \hat{x} + \beta$.
- **RMSNorm** — the simplified version: rescale by the RMS with no re-centring and no bias. Equally effective in practice, and faster.
- **Pre-norm vs post-norm** — normalising before the sub-block (pre-norm, stable at depth) vs after (post-norm, unstable beyond ~12 layers).
- **Bias absorption** — re-centring normalisation mathematically cancels any [[bias.en|bias]] added before it, making such biases redundant.
- **Smoothing the loss landscape** — the main empirical benefit: normalisation makes training more stable and tolerates larger learning rates.

## Deep dive

### The problem: activation drift

Without normalisation, the activations of a deep network tend to **drift** across layers: either they **grow** exponentially (each layer multiplies by $> 1$ on average), or they **shrink** exponentially. Over 80 layers, a multiplicative factor of 1.05 per layer gives $1.05^{80} \approx 50$ — already a problem. It is worse still for gradients, which pass back through all those layers (vanishing/exploding gradients).

Normalisation forces signal stability at every step:

1. Stabilises the **activations** in the forward pass.
2. Stabilises the **gradient** in the backward pass.
3. **Smooths the loss landscape** (the empirical result of *How does batch normalization help optimization?*, Santurkar et al. 2018: this is the real mechanism, not the reduction of "covariate shift" as was previously believed).
4. Allows **larger learning rates** → faster training.

### LayerNorm in detail

For an activation vector $x \in \mathbb{R}^d$ (a single token, dimension $d_{\text{model}}$):

**Step 1 — re-centre and normalise**:

$$\mu = \frac{1}{d}\sum_{i=1}^{d} x_i \qquad \sigma^2 = \frac{1}{d}\sum_{i=1}^{d} (x_i - \mu)^2$$

$$\hat{x}_i = \frac{x_i - \mu}{\sqrt{\sigma^2 + \varepsilon}}$$

After this step the vector has **mean 0 and variance 1**, whatever it was before. The $\varepsilon \sim 10^{-5}$ avoids division by zero.

**Step 2 — learned affine transform**:

$$y_i = \gamma_i \cdot \hat{x}_i + \beta_i$$

With two learned vectors:
- $\gamma$ (gain) → rescales each dimension independently.
- $\beta$ (the norm's bias) → shifts each dimension independently.

**Why step 2?** Without it, all activations would be forced to mean 0, variance 1 — destroying useful information the network may have wanted to put at different scales. With $\gamma$ and $\beta$, the network can **undo** the normalisation if it wants ($\gamma = \sigma, \beta = \mu$), **keep it as is** ($\gamma = 1, \beta = 0$), or anything in between. Normalisation becomes an adjustable option, not a rigid constraint.

Cost: $2d$ parameters per LayerNorm. For a Transformer with $d_{\text{model}} = 8192$ and ~200 LayerNorms: ~3.3M parameters. Negligible.

### RMSNorm: the modern simplification

Used by Llama, Mistral, and most recent models. It is LayerNorm **without the re-centring and without the bias**:

$$\text{rms}(x) = \sqrt{\frac{1}{d}\sum_{i=1}^{d} x_i^2}$$

$$y_i = \gamma_i \cdot \frac{x_i}{\text{rms}(x) + \varepsilon}$$

Differences from LayerNorm:
- **No mean computation** → slightly faster (fewer reductions).
- **No subtraction** $x_i - \mu$ → no re-centring, only rescaling.
- **No bias** $\beta$ → fewer parameters.

Why does it work just as well? An empirical finding (Zhang & Sennrich, *Root Mean Square Layer Normalization*, 2019): LayerNorm's benefit comes mainly from **rescaling**, not from re-centring. Once the norm is controlled, centring is marginal. Result: ~7-10% faster to compute for identical quality.

### Pre-norm vs post-norm — a crucial detail

The original Transformer (2017) placed LayerNorm **after** each sub-block — **post-norm**:

$$x_{\text{out}} = \text{LayerNorm}(x_{\text{in}} + \text{sublayer}(x_{\text{in}}))$$

But empirically that is **unstable** beyond ~12 layers (a delicate warmup is required).

Modern models use **pre-norm**: normalisation **before** the sub-block:

$$x_{\text{out}} = x_{\text{in}} + \text{sublayer}(\text{LayerNorm}(x_{\text{in}}))$$

**The crucial difference**: in pre-norm, the **residual stream** ($x_{\text{in}}$ added to the output) is **never normalised**. It carries information through every layer without distortion. That is what makes it possible to train networks of 80, 120, 200 layers stably.

```
Post-norm (original, unstable at depth):
  x_in ─┬─→ sublayer ─→ + ─→ LayerNorm ─→ x_out
        │                ↑
        └────────────────┘

Pre-norm (modern, stable):
  x_in ─┬─→ LayerNorm ─→ sublayer ─→ + ─→ x_out
        │                            ↑
        └────────────────────────────┘
```

In pre-norm: one LayerNorm per sub-block (hence 2 per [[transformer-architecture.en|Transformer]] layer — one before [[attention-mechanism.en|attention]], one before the MLP), plus a **final LayerNorm** before the un-embedding.

### How normalisation absorbs the bias's role

This is the key connection with [[bias.en|the bias]].

Without normalisation, in a layer $y = Wx + b$:
- $W$ learns **how** to combine the inputs.
- $b$ learns **at what baseline level** to place the output.

With LayerNorm right after (post-norm) or right before the next layer (pre-norm), the re-centring cancels the mean of the activations. Mathematically:

$$\text{if } z = Wx + b, \text{ then } z - \text{mean}(z) = Wx - \text{mean}(Wx)$$

**The $b$ cancels.** Keeping the bias in the linear layer is therefore strictly redundant with LayerNorm. That is why Llama (RMSNorm pre-norm throughout) has **no biases at all** in its linear layers: they would add nothing.

For RMSNorm (no re-centring), a bias would not be strictly cancelled, but $\gamma$ already plays the role of per-dimension scaling, and practice shows you can do without it at no cost.

### BatchNorm / LayerNorm / RMSNorm compared

|                   | **BatchNorm**                                              | **LayerNorm**                        | **RMSNorm**             |
|-------------------|------------------------------------------------------------|--------------------------------------|-------------------------|
| Normalises over   | the batch dimension (at fixed feature)                     | the feature dimension (at fixed token) | same as LayerNorm     |
| Computes mean+var | yes (over the batch)                                       | yes (over the features)              | no (just the RMS)       |
| Affine            | $\gamma, \beta$                                            | $\gamma, \beta$                      | $\gamma$ only           |
| Suited to         | CNNs, large fixed batches                                  | sequences, variable batches          | modern Transformers     |
| Drawback          | depends on the batch — a problem for single-sample inference | none major                          | none major              |

BatchNorm (2015) unlocked deep CNNs. LayerNorm (2016) is its variant for sequential models (RNNs then Transformers). RMSNorm (2019) is the minimalist simplification that dominates from 2024 onwards.

## Examples & analogies

- **The network's pacemaker.** Without normalisation, the cardiac signal (the activations) goes arrhythmic with depth — either tachycardia (explosion) or bradycardia (extinction). Normalisation imposes a controlled rhythm at every layer, letting information circulate without racing away or dying out.
- **An imposed system of units.** Imagine a workshop where every worker measures in a different unit depending on their mood: centimetres, inches, kilometres. Unmanageable. Normalisation means imposing SI units at every step — the workers can still do what they like (via $\gamma, \beta$), but on a common basis.

## Open questions

- The exact mechanism by which normalisation smooths the loss landscape remains poorly understood theoretically.
- Are there performant deep architectures with **no** normalisation at all? (cf. ReZero, SkipInit — mixed results)
- Why can LayerNorm's re-centring be dropped at no cost by RMSNorm, when intuitively it ought to contribute stability?

## Related notes

- [[bias.en]]
- [[transformer-architecture.en]]
- [[attention-mechanism.en]]
- [[how-neural-networks-train.en]]
