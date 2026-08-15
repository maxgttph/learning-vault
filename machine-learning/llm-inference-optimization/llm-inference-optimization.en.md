---
title: "LLM Inference: KV Cache and TurboQuant"
aliases: ["KV cache", "TurboQuant", "LLM inference"]
tags: [llm, inference, optimization, kv-cache, quantization]
created: 2026-05-26
source: conversation with Claude (TurboQuant ICLR 2026, PolarQuant AISTATS 2026)
lang: en
translations:
  - "[[llm-inference-optimization.fr]]"
related: ["[[attention-mechanism.en]]", "[[transformer-architecture.en]]", "[[recurrent-networks-and-lstm.en]]", "[[distillation.en]]", "[[mixture-of-experts.en]]"]
---

# LLM Inference: KV Cache and TurboQuant

## TL;DR

> LLM inference is dominated by **memory**, not compute. The **KV cache** avoids recomputing the K and V vectors of past tokens at every generation step, bringing the per-token cost from $O(n^2)$ down to $O(n)$. **TurboQuant** (Google, ICLR 2026) compresses that cache 6× with no measurable loss and no retraining.

## Key concepts

- **KV cache** — storage of the Key and Value vectors computed for past tokens, reused at every newly generated token.
- **Quantization** — reducing numerical precision (16 → 8 → 4 → 3 bits) to save memory and compute.
- **GQA (Grouped Query Attention)** — several Q heads share the same K/V → shrinks the cache. Used by Llama 3.
- **MQA (Multi-Query Attention)** — the extreme version: a single K/V for all Q heads.
- **TurboQuant** — Google's algorithm compressing the KV cache to 3 bits/element (a 6× reduction) losslessly, with up to 8× attention speedup on H100.
- **PagedAttention** — OS-page-style memory management for the cache (popularised by vLLM).

## Deep dive

### The problem

To generate token $N+1$, [[attention-mechanism.en|attention]] has to look at all tokens $1 \ldots N$. Naively, you must recompute K and V for each of those tokens at every generation step → a cost of $O(n^2)$ per token, hence $O(n^3)$ to generate $n$ tokens. Catastrophic on long contexts.

### The solution: the KV cache

The key observation: thanks to the **causal mask**, the K and V of past tokens **do not depend on the new token** — a past token does not change because of what comes after. So they can be memorised:

- For token $N+1$: you only compute its $Q$, $K$, $V$ (and append K, V to the cache).
- The new Q attends against **the entire existing cache**.
- Cost per token: $O(n)$ instead of $O(n^2)$.

To generate 1000 tokens with a 10,000-token context, this is the difference between seconds and hours.

### The new bottleneck: memory

The KV cache grows with the context. For Llama 3 70B on an 8000-token context: **several GB of VRAM** per request, on top of the model itself.

That is why LLMs "blow up in memory" on long contexts: not so much because of the model (constant size), but because of the cache, which grows linearly with context AND with the number of concurrent requests.

Hence a flowering of optimisations:

- **GQA (Grouped Query Attention)** — several Q heads share their K/V. Llama 3 uses it.
- **MQA (Multi-Query Attention)** — the extreme version: one K/V for all heads.
- **Sliding window** — keep only the last N tokens in the cache (Mistral).
- **PagedAttention** — page-based memory management, like an OS, for efficient reuse (vLLM).

### TurboQuant (Google, ICLR 2026)

Compresses the KV cache to **3 bits per element** (vs 16 bits in FP16) → a **6×** reduction, with no measurable loss, **without retraining** or calibration data. Pure post-processing at inference time.

**How it works** (two stages):

1. **PolarQuant** (AISTATS 2026, the first building block) — applies a **random orthogonal rotation** to the K and V vectors before quantisation. Why: classic "MSE-optimal" scalar quantisers introduce a systematic bias in the estimate of the dot product $Q \cdot K^\top$ — which is exactly what attention needs. The random rotation spreads the error across all directions, eliminating the bias.

2. **1-bit QJL correction** — quantises the residual (remaining error) on one extra bit, bringing it even closer to the original value.

**Practical impact:**

- At 3.5 bits: quality identical to FP16 (Needle score 0.997).
- Up to **8× speedup** of attention on H100 (attention is *memory-bound* — less memory read = faster).
- Enables: 6× longer contexts at constant memory, OR 6× more concurrent users on the same hardware, OR running large models on modest hardware.

This is typical of the "systems" progress that accumulates alongside "architectural" progress: models do not merely get bigger, we also learn to run them ever more efficiently.

## Examples & analogies

- **The cache = summaries of the chapters you've read.** Without a KV cache, it is like re-reading every chapter of a book for each new paragraph you write. With it, you keep a synthetic summary of the previous chapters to hand, and only have to integrate the new paragraph.
- **PolarQuant's random rotation.** Comparable to systematically scrambling a grid's orientation before rounding points onto it: the rounding error spreads across all directions instead of aligning with the grid, which destroys directional biases.

## Open questions

- What gains are left in KV cache compression? Is sub-2-bit conceivable?
- How can TurboQuant be combined with PagedAttention in production (vLLM, TGI)?
- At what point does compressing the model (weights) become the new bottleneck, relative to the cache?

## Related notes

- [[attention-mechanism.en]]
- [[transformer-architecture.en]]
- [[recurrent-networks-and-lstm.en]]
- [[distillation.en]]
- [[mixture-of-experts.en]]
