---
title: "The Transformer Architecture"
aliases: ["transformer", "GPT architecture"]
tags: [machine-learning, transformer, llm, architecture]
created: 2026-05-26
source: conversation with Claude
lang: en
translations:
  - "[[transformer-architecture.fr]]"
related: ["[[neural-networks-overview.en]]", "[[how-neural-networks-train.en]]", "[[attention-mechanism.en]]", "[[embeddings-and-tokenization.en]]", "[[llm-inference-optimization.en]]", "[[normalization.en]]", "[[bias.en]]", "[[recurrent-networks-and-lstm.en]]", "[[reinforcement-learning.en]]", "[[world-models.en]]", "[[distillation.en]]", "[[mixture-of-experts.en]]"]
---
# The Transformer Architecture

## TL;DR

> The Transformer (Vaswani et al., *Attention Is All You Need*, 2017) is the architecture that dominates modern LLMs. It stacks identical blocks (attention + MLP, wrapped in normalisations and residual connections) over sequences of tokens represented as vectors. Its strength: it is parallelisable and captures long-range dependencies without signal degradation.

## Key concepts

- **Transformer layer** — an identical sub-block, repeated 32 to 100+ times. Contains one attention + one MLP, with normalisations and residuals around them.
- **Residual connection** — $x + f(x)$ instead of $f(x)$. Stabilises training and lets the gradient flow back easily.
- **LayerNorm** — normalises activations layer by layer to avoid explosion/vanishing.
- **MLP / FFN** — the feed-forward block, home to ~60–70% of the parameters. An associative memory storing the model's "facts".

## Deep dive

### The full LLM pipeline

```
text
 → tokenization (BPE) → IDs
 → embedding → one vector per token
 → + position encoding (RoPE)
 → N Transformer layers
 → un-embedding → distribution over the vocabulary
 → softmax → probabilities
 → sampling → next token
 → loop (autoregressive)
```

See [[embeddings-and-tokenization.en]] for the input and output, and [[attention-mechanism.en]] for the heart of a layer.

### Anatomy of a layer

A layer is not just "matrix × vector": it is a sub-graph with normalisations and residuals.

```
x_in
 ├─ (kept for the residual)
 → LayerNorm
 → Attention (Q, K, V, O)
 → + x_in                    ← first residual
 ├─ (kept)
 → LayerNorm
 → MLP (W_up, W_gate, W_down)
 → + previous                ← second residual
x_out
```

### The MLP: associative memory

The key difference from attention:
- **Attention** mixes information **between tokens**.
- **The MLP** transforms each token **independently**.

Classic form (GPT-2, BERT):

$$y = W_{\text{down}} \cdot \text{activation}(W_{\text{up}} \cdot x + b)$$

Typical hidden dimension: $4 \times d_{\text{model}}$. Activations: GELU, SiLU.

Modern form (Llama, SwiGLU):

$$y = W_{\text{down}} \cdot \big(\, \text{SiLU}(W_{\text{gate}} \cdot x) \;\odot\; (W_{\text{up}} \cdot x) \,\big)$$

(where $\odot$ is the element-wise product.)

Recent research suggests MLPs act as a **giant associative memory**: each hidden neuron detects a particular pattern and activates a response. This is where the "facts" live ("Paris is the capital of France"), not in attention.

### Why several layers

- **Without a non-linearity in between, several layers equal one**: $W_2 \cdot (W_1 \cdot x) = (W_2 \cdot W_1) \cdot x = W' \cdot x$. Without ReLU/GELU/SiLU interleaved, GPT-4 would be a linear regression.
- **Composition of operations**: 32 layers = 32 successive refinement steps.
- **Empirically observed hierarchy**: lower layers → syntax, middle → semantics, upper → task-specific.
- **Computational depth** = available reasoning depth.

### Why the Transformer changed everything (2017)

- **Parallelisable** — all tokens compute their Q/K/V in parallel during training (vs. a sequential RNN). Huge on GPUs.
- **Long-range dependencies without degradation** — any token can look at any other directly through [[attention-mechanism.en|attention]], whatever the distance.
- **Scaling laws** — predictable performance with size. That justified the massive investment.

### The price of parallelism: positional encoding

Processing all tokens "at once" has a hidden cost. Attention computes scores between pairs of tokens via $Q\,K^\top$, but that operation **contains no notion of order**: permuting the input tokens merely permutes the outputs (attention is *permutation-equivariant*). To it, "the cat eats the mouse" and "the mouse eats the cat" are **indistinguishable**.

An [[recurrent-networks-and-lstm.en|RNN]] does not have this problem: it knows the order **for free**, because it walks through the words one by one — position is implicit in the unrolling of the loop. The Transformer sacrificed that sequential walk to gain parallelisation; it must therefore **reinject positional information artificially**, through positional encoding (sinusoidal originally, **RoPE** in modern LLMs).

This is a genuine structural subtlety, not an implementation detail: positional encoding exists **only** because attention is blind to order. No recurrence ⇒ you have to tell the model explicitly "this token is at position 5".

### Typical parameter breakdown

| Component | Share of parameters |
|---|---|
| MLP | ~60–70% |
| Attention (Q, K, V, O) | ~25–35% |
| Embeddings + the rest | a few % |

Attention is the conceptual innovation, but most of the model lives in the MLPs.

## Examples & analogies

- **Attention = "what to think about", MLP = "what I know about it".** This dichotomy helps remember the two roles.
- **Layers as refinement steps**: to understand "The cat that Marie adopted yesterday is sleeping", you first have to recognise the words, then resolve the grammatical links ("that" → "cat"), then disambiguate, then integrate. A single layer would not do it.

## Open questions

- What alternatives to the Transformer are emerging? (Mamba, RWKV, state-space models)
- Can the quadratic complexity of attention be beaten without losing quality?
- What is the natural limit of the scaling laws — is there an end?

## Related notes

- [[neural-networks-overview.en]]
- [[how-neural-networks-train.en]]
- [[attention-mechanism.en]]
- [[embeddings-and-tokenization.en]]
- [[llm-inference-optimization.en]]
- [[normalization.en]]
- [[bias.en]]
- [[recurrent-networks-and-lstm.en]]
- [[reinforcement-learning.en]]
- [[world-models.en]]
- [[distillation.en]]
- [[mixture-of-experts.en]]
