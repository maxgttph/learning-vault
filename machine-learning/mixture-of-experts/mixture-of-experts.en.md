---
title: "Mixture of Experts (MoE)"
aliases: ["mixture of experts", "MoE", "sparse MoE"]
tags: [machine-learning, transformer, llm, architecture, sparsity]
created: 2026-06-16
source: conversation with Claude
lang: en
translations:
  - "[[mixture-of-experts.fr]]"
related: ["[[transformer-architecture.en]]", "[[llm-inference-optimization.en]]", "[[how-neural-networks-train.en]]", "[[attention-mechanism.en]]", "[[distillation.en]]"]
---

# Mixture of Experts (MoE)

## TL;DR

> **Mixture of Experts** is not a rival architecture to the Transformer: it is a modification **inside** the Transformer. It replaces each block's dense MLP with **N small MLPs (the experts) + a router** that activates only a subset (top-k) per token. The result: it **decouples the model's total knowledge (total parameters) from its compute cost per token (active parameters)**.

## Key concepts

- **Expert** — a plain MLP (the $W_{\text{up}}/W_{\text{gate}}/W_{\text{down}}$ block). Not a full Transformer: no attention, no embeddings, no layers of its own.
- **Router (gating network)** — a small linear layer that scores the experts and selects the **top-k** for each token.
- **Top-k routing** — only the $k$ best-scoring experts are activated (typically $k=2$ out of 8, 64 or 256 experts).
- **Total vs active parameters** — all experts occupy memory (total, huge); only $k$ run per token (active, small).
- **Conditional / sparse computation** — each token traverses only a fraction of the network, chosen dynamically.
- **Load-balancing loss** — an auxiliary loss forcing the router to spread tokens fairly, otherwise it collapses onto a few experts.

## Deep dive

### What MoE changes (and does not)

A model is "a Transformer" by its overall architecture: the stack of identical blocks, each containing **attention + MLP**, wrapped in normalisations and residuals (see [[transformer-architecture.en]]). MoE replaces **none** of that. It modifies **one single component**: each block's dense MLP becomes a set of expert-MLPs + a router.

The correct phrasing is therefore: Mixtral, DeepSeek, GPT-4 (and so on) are **Transformers that use MoE layers**. "MoE" is an adjective on the FFN, not a competing architecture. It is never "Transformer **or** MoE" — it is always both at once.

Two elements stay **dense and shared by every token**:
- **attention** (every token always attends through the same $Q/K/V$);
- the residual/normalisation plumbing.

Only the MLP gets "expertised".

```
Transformer block (MoE variant):
  x
  → LayerNorm
  → Attention          ← UNCHANGED, dense, shared by all tokens
  → + residual
  → LayerNorm
  → Router picks top-k experts ┐
       Expert 1 (an MLP)       │  ← only the MLP is split
       Expert 2 (an MLP)       │
       ... Expert N (an MLP)   ┘
  → + residual
  x_out
```

The split is **per layer**: a 32-layer model has 32 independent sets of experts and 32 independent routers. Routing is **re-decided at every layer** — a token can go to expert 3 in layer 5, then expert 7 in layer 6.

### Why MoE attacks the MLP specifically

Per [[transformer-architecture.en]], the MLP concentrates ~60–70% of the parameters and plays the role of **associative memory** (that is where the "facts" live). But in a dense Transformer, **every token activates all of those weights**, even when the overwhelming majority of stored "facts" are irrelevant: a token about French cooking lights up the same neurons as a token about Python syntax. MoE removes that waste by activating only the relevant experts.

### The router's mechanism

The router is tiny: $\text{logits} = W_{\text{router}} \cdot x$, then softmax, then keep the $k$ best. The MoE layer's output is the **weighted sum** of the chosen experts' outputs, weighted by their softmax scores:

$$y = \sum_{i \in \text{top-}k} g_i(x)\, E_i(x), \qquad g(x) = \text{softmax}(W_{\text{router}}\, x)$$

where $E_i$ is expert $i$ and $g_i$ its gating weight.

### How the router is trained

No labels saying "this token should go to expert 4", no separate training. The router is learned **end-to-end by the same backprop** as the rest of the model (see [[how-neural-networks-train.en]]).

The mechanism: because the chosen experts' output is **multiplied by the router's softmax weights**, the gradient flows back to $W_{\text{router}}$. If expert 2 produced a useful result, the loss falls, and the gradient pushes $W_{\text{router}}$ to give expert 2 a higher score for similar tokens. **Specialisation is emergent, never assigned** — exactly the forced-divergence dynamic described in [[attention-mechanism.en]] (the "three identical workers" who specialise through their position in the chain).

### Collapse and the balancing loss

Left alone, that router collapses through self-reinforcement: an expert that is slightly better at the start receives more tokens → trains more → gets even better → captures *all* the tokens. The others starve (*rich get richer*).

To prevent it, an **auxiliary balancing loss** is added to the main loss:

$$\mathcal{L} = \mathcal{L}_{\text{main}} + \alpha \cdot \mathcal{L}_{\text{balance}}$$

with a small $\alpha$ (≈ 0.01). $\mathcal{L}_{\text{balance}}$ measures the inequality of token distribution across experts and penalises it. The router therefore feels **two simultaneous pressures**: the main loss pushes it towards *useful* routing (send the token where it helps), the balancing loss towards *fair* routing (do not starve any expert). The final router is the compromise.

Auxiliary mechanisms: **noise** on the router's logits (*noisy top-k gating*) to encourage exploration, and a per-expert **capacity** cap (surplus tokens are *dropped* or pass through unchanged via the residual).

### The real cost: memory, not compute

MoE moves the bottleneck exactly into the regime described in [[llm-inference-optimization.en]]: inference is **memory-bound, not compute-bound**.

- **You save FLOPs, not memory.** All experts must live in VRAM even if only $k$ run. A "13B active" model keeps the memory footprint of a 47B.
- **Communication**: experts are often spread across several GPUs (*expert parallelism*), so routing = transferring tokens between devices.
- At inference the router is **frozen**: it just does top-k and executes. Hence the advantage (low compute per token) and the price (the memory cost of every expert).

## Examples & analogies

- **Mixtral 8×7B**: ~47 billion total parameters, but only ~13 billion activated per token (top-2 out of 8 experts).
- **The hospital and the triage nurse.** A dense model is a single GP examining every patient with all of their knowledge — exhaustive but slow and wasteful. An MoE is a hospital with a **triage nurse (the router)** sending each patient to the 2 most relevant specialists out of 64. The hospital collectively knows far more (huge total capacity), but each patient only occupies two specialists (low active compute). The price: you still have to **pay all 64 specialists to be there** (memory), and the nurse had better triage well.

## Open questions

- How does MoE reshape the **scaling laws**? (growth in total capacity without proportional growth in per-token compute)
- **Fine-grained MoE** vs **large experts**: DeepSeek-V3 and its hundreds of small experts + always-active "shared" experts — what is the trade-off?
- MoE × KV cache **quantization**: where does the real memory bottleneck sit then?
- Can a large MoE be **distilled** into a smaller dense model, and what is lost? (see [[distillation.en]])

## Related notes

- [[transformer-architecture.en]]
- [[llm-inference-optimization.en]]
- [[how-neural-networks-train.en]]
- [[attention-mechanism.en]]
- [[distillation.en]]
