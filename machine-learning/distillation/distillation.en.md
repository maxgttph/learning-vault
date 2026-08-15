---
title: "LLM Distillation"
aliases: ["distillation", "knowledge distillation"]
tags: [machine-learning, llm, distillation, training, post-training]
created: 2026-06-16
source: conversation with Claude
lang: en
translations:
  - "[[distillation.fr]]"
related: ["[[how-neural-networks-train.en]]", "[[embeddings-and-tokenization.en]]", "[[bias.en]]", "[[llm-inference-optimization.en]]", "[[transformer-architecture.en]]", "[[mixture-of-experts.en]]"]
---

# LLM Distillation

## TL;DR

> Distillation trains a small **student** model to imitate a large **teacher** model, transferring its competence into a cheaper network. The key signal is not the "right answer" but the teacher's **full distribution** over the vocabulary (the *soft targets*) — which encodes far more information than a single target token.

## Key concepts

- **Logits** — the raw scores produced by the un-embedding, one per vocabulary token, before the softmax. Unbounded reals; only their **differences** matter.
- **Soft targets / dark knowledge** — the teacher's probability distribution (not just its top-1). Encodes *which wrong answers are plausible*.
- **Temperature $T$** — a factor applied to the logits before the softmax ($z_i / T$) to flatten the distribution and make small probabilities visible.
- **Logit distillation** — the student imitates the teacher's softmaxed distribution (KL loss). Requires access to the teacher's logits.
- **Sequence-level distillation** — the teacher *generates* text and the student is fine-tuned on it with classic next-token loss. No logits needed. The dominant method in practice.
- **Ceiling** — the student converges towards the **minimum** of the teacher's capability and its own capacity; never beyond.

## Deep dive

### Where distillation sits in training

The word *training* is an umbrella, not a phase: any weight update through the forward → loss → backprop → optimiser loop (see [[how-neural-networks-train.en]]). What distinguishes the phases is **the data and the loss**.

- **Pre-training** (~90% of the compute) — next-token prediction on massive raw text (Llama 3: 15 trillion tokens), with no annotation. Builds **knowledge** and linguistic competence.
- **Post-training** — everything after. Adds almost no knowledge; it **sculpts behaviour** out of what pre-training learned:
  - **SFT** — fine-tuning on `(instruction, good answer)` pairs, same next-token loss. Teaches the "assistant" format.
  - **Alignment (RLHF / DPO)** — a preference loss (not next-token), from rankings of answers.
  - **Distillation** — transfers the teacher's *behaviour*. Structurally close to SFT (if you fine-tune on generated text) or a distinct objective (if you match logits with KL + temperature).

Mental model: **pre-training pours in the knowledge, post-training sculpts it into a usable shape.** Distillation sculpts the student to resemble the teacher.

### Why soft targets are worth more than hard labels

An LLM's standard loss is a cross-entropy against **the true next token** — a single right answer ("hard label"). For "The capital of France is ___", the target is `Paris`, probability 1, everything else 0.

But a trained teacher produces a **full distribution** through its logits (see the un-embedding in [[embeddings-and-tokenization.en]]):

```
Paris 0.90, Lyon 0.04, the 0.02, Marseille 0.01, banana 1e-7, ...
```

That distribution says *which errors are near-misses* (`Lyon` plausible, `banana` absurd). It is this structure — the **dark knowledge** — that we want to transfer to the student.

### The role of temperature

The problem: a confident teacher puts almost all the mass on `Paris` (0.90+), crushing the interesting information (`Lyon` 0.04 vs `banana` 1e-7) down near zero. In the loss those tiny differences weigh almost nothing — the dark knowledge is wasted.

**Temperature flattens the distribution** to make those small probabilities usable. Each logit is divided by $T$ before the softmax:

$$p_i = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$$

- $T = 1$ → normal softmax.
- $T > 1$ (e.g. 3–4) → shrinks the **gaps** between logits → a flatter distribution. `Paris` goes from 0.90 to ~0.50, `Lyon` from 0.04 to ~0.12, `banana` from 1e-7 to ~1e-4. The relative structure is amplified and learnable.
- $T \to \infty$ → uniform distribution. $T < 1$ → more peaked.

A reminder: the softmax depends only on logit **differences**. Dividing by $T = 3$ shrinks every gap by a factor of 3 — exactly "making the distribution less peaky".

You run teacher **and** student at the same temperature, and the distillation loss pulls the student's distribution towards the teacher's (KL divergence). Often combined with the hard-label cross-entropy:

$$L = \alpha \cdot L_{\text{distill}}(T) \cdot T^2 + (1 - \alpha) \cdot L_{\text{CE}}(\text{true token})$$

The $T^2$ term rescales the gradients that flattening shrinks. After training you throw the temperature away and the student runs at $T = 1$.

> **An important nuance**: this method (*logit distillation*) requires access to the teacher's logits. Many models labelled "distilled" today do **sequence-level distillation** instead: the teacher *generates* text and the student is fine-tuned on it with classic next-token loss (no temperature, no KL). That is training on synthetic data.

### Convergence: does the student catch up with the teacher?

The principle: the teacher's output is the **only signal**, so the teacher is a **ceiling** — the student converges *towards* it, never *beyond* it (in pure distillation). The student's capacity is a second ceiling. The student tends towards **the lower of the two**.

- **Same-size student** — in principle it can approach the teacher (enough capacity to represent the same function), over the sampled prompt distribution. But it *matches* at best, never exceeds; and it diverges out of distribution. Rarely useful in practice.
- **Smaller student** (the normal case) — **plateaus strictly below the teacher.** Its limited capacity is the binding ceiling. More tokens help it *reach* the plateau (with log-diminishing returns), then stop helping. A 7B distilled from a 70B closes *part* of the gap, not all of it.
- **Larger student** — converges to the teacher **and stops there**: the excess capacity buys nothing, because the teacher is the ceiling. Worse, the surplus can **overfit the teacher's flaws and errors** (it inherits its biases, see [[bias.en]]). To *exceed* the teacher, you need a signal beyond it: ground truth, RL, self-improvement.

In short: accuracy $\to \min(\text{teacher ceiling},\ \text{student capacity ceiling})$.

### Why you start from an already pre-trained base model

A common intuition: "if backprop comes from the teacher's outputs, the base model only speeds things up, it doesn't make the result smarter." True in **one limit**, false in **the regime that matters**.

- **The infinite-data limit**: with infinite, fully covering teacher data, the starting point washes out — gradient descent would sculpt the same function from random weights. There the base only saves time.
- **The real regime (finite data)**: a distillation set runs to ~1 billion tokens / ~1M examples, i.e. **thousands of times less** than pre-training (15T). That signal is far too sparse to rebuild general knowledge from scratch. The teacher only demonstrates its behaviour on the sampled prompts; everywhere else, a randomly initialised student would be incompetent. **The base fills exactly those gaps** with its own pre-training knowledge.

So: final competence = (the student's pre-trained knowledge) + (the teacher's distilled behaviour). The first term is enormous and irreplaceable at real data scale → the base is **load-bearing, not a mere accelerator**. That is why a better base model gives a better student at equal teacher data — which would not be true if the base only saved time.

### How many tokens for near-teacher performance?

No single number, but orders of magnitude:

- Far smaller than pre-training: typically **~100k to ~1M examples** (hundreds of millions to a few billion tokens). DeepSeek-R1: ~800k generated samples.
- You rarely *match* the teacher at equal budget — distillation makes a small model punch above its weight, without closing everything.
- Fast diminishing returns (log-shaped): the first 100k examples buy a lot, the rest less and less. The **diversity/coverage** of prompts matters more than raw volume.

The base model is often the dominant factor: minimum capacity (a floor), knowledge already present, teacher–student alignment (architecture, **tokenizer** — a different tokenizer breaks logit distillation, see [[embeddings-and-tokenization.en]]), and the inherited ceiling/biases.

### Distillation vs other compressions

- **Quantization** — reduces the numerical precision of the weights (see [[llm-inference-optimization.en]]). Same architecture, fewer bits.
- **Pruning** — removes weights/neurons/heads.
- **Distillation** — trains a *different*, smaller model to imitate the behaviour.

They are complementary: you often distil *then* quantise.

## Examples & analogies

- **DistilBERT** — ~40% smaller, ~60% faster, ~97% of BERT's performance.
- **Distilled DeepSeek-R1** — reasoning traces from a large R1 teacher used to fine-tune small Qwen/Llama students, transferring chain-of-thought ability.
- **Soft targets = an annotated answer key.** A hard label just says "the answer is Paris"; soft targets are the teacher also annotating *why Lyon is an understandable mistake and banana is nonsense*. The student learns the nuance, not just the answer.
- **The pre-trained base = the student's general education.** Distillation is only a thin behavioural layer on top; without the base, the teacher's signal is too sparse to rebuild the knowledge.

## Open questions

- When should logit distillation (KL + temperature) be preferred to sequence-level (synthetic data)? The logit-access / quality trade-off.
- **On-policy distillation**: having the teacher correct the student's own samples — what are the gains over classic distillation?
- How can the teacher's ceiling be exceeded? (combining distillation + RL + ground truth).
- How far does distillation propagate the teacher's biases, and how can they be filtered?

## Related notes

- [[how-neural-networks-train.en]]
- [[embeddings-and-tokenization.en]]
- [[bias.en]]
- [[llm-inference-optimization.en]]
- [[transformer-architecture.en]]
- [[mixture-of-experts.en]]
