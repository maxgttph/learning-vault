---
title: "Embeddings and Tokenization"
aliases: ["embedding", "tokenizer", "BPE"]
tags: [machine-learning, embeddings, tokenizer, llm]
created: 2026-05-26
source: conversation with Claude
lang: en
translations:
  - "[[embeddings-and-tokenization.fr]]"
related: ["[[transformer-architecture.en]]", "[[attention-mechanism.en]]", "[[recurrent-networks-and-lstm.en]]", "[[world-models.en]]", "[[distillation.en]]"]
---

# Embeddings and Tokenization

## TL;DR

> Raw text is first cut into **tokens** by a separate algorithm (BPE), trained ahead of the model. Each token is turned into a vector through an **embedding matrix** learned by backpropagation — nobody decides by hand which vector corresponds to which word. At the output, an **un-embedding** projects back onto the vocabulary to produce next-token probabilities.

## Key concepts

- **Token** — the basic unit an LLM manipulates, typically a sub-word (somewhere between a letter and a whole word).
- **Tokenizer** — the algorithm that cuts up raw text. Trained separately, ahead of the model. Frozen thereafter.
- **BPE (Byte Pair Encoding)** — iteratively merges the most frequent character pairs. Used by GPT, Llama.
- **Embedding matrix** — a lookup table of size $|\text{vocab}| \times d_{\text{model}}$, **learned** like every other weight.
- **Un-embedding** — a $d_{\text{model}} \times |\text{vocab}|$ matrix projecting the model's output onto vocabulary scores. Often tied to the embedding.

## Deep dive

### Tokenization comes first

Raw text is cut into tokens first. The two dominant algorithms:

- **BPE (Byte Pair Encoding)** — used by GPT, Llama. Starts from individual characters and iteratively merges the most frequent pairs, until it reaches the target vocabulary size (typically 30,000 to 200,000 tokens).
- **SentencePiece / Unigram** — used by some Google models.

The tokenizer is trained BEFORE the model, separately, from the corpus. Once frozen, it becomes inseparable from the model: changing the tokenizer = starting over.

### The embedding is just a lookup

A matrix $E$ of size $|\text{vocab}| \times d_{\text{model}}$. For $d_{\text{model}} = 8192$ and a 128k vocabulary: ~1 billion parameters for that matrix alone.

Token ID 42 → row 42 of $E$. **No computation, just a read.**

**The crucial point**: nobody decides what corresponds to what.
- The matrix is **randomly initialised** (Gaussian $\mathcal{N}(0,\ 0.02)$).
- It is **trained by backpropagation** like every other weight.

By the end of training, properties emerge automatically:
- Words with similar contexts → geometrically close vectors.
- $\text{Paris} - \text{France} \approx \text{Berlin} - \text{Germany}$ (the famous vector arithmetic).

This is pure self-organisation, driven by the optimisation pressure of the loss (predicting the next token).

### The un-embedding closes the loop

At the end of the forward pass, you have a vector of dimension $d_{\text{model}}$ (the final representation of the last token). You multiply it by a $d_{\text{model}} \times |\text{vocab}|$ matrix, giving **one logit per possible token** in the vocabulary. A softmax turns that into a probability distribution → you sample the next token.

**A common trick**: many models (GPT-2, some Llamas) **tie** the weights of embedding and un-embedding ($E$ and $E^\top$). It saves parameters and has a theoretical justification (the symmetry of the token encode/decode operation).

### Why changing the tokenizer breaks everything

Token IDs are **arbitrary** — defined purely by the tokenizer. If you change:

- **The whole tokenizer** → the IDs change, so the entire embedding matrix becomes nonsense, and so does the whole model. You have to **retrain everything**.
- **Only the embedding matrix** (same tokenizer) → the rest of the model no longer knows what the input vectors mean. At a minimum, retrain the neighbouring layers; in practice, the whole model.

That is why a Llama download always ships with its tokenizer attached: they form an inseparable pair.

### Position encoding (an aside)

Without positional information, attention would treat "the cat sleeps" and "sleeps the cat" identically. So a **positional encoding** is added to the embedding. Llama and Mistral use **RoPE** (Rotary Position Embedding), which encodes position through a rotation applied to the Q and K vectors in [[attention-mechanism.en|attention]].

## Examples & analogies

- **An index in a giant dictionary.** The embedding does not learn "what a cat is" — it learns where in geometric space to place the "cat" vector so that the following layers manipulate it correctly.
- **The tokenizer / model pair.** Comparable to a lock and its key: the key (model) makes no sense without the lock (tokenizer) that defines the expected geometry. Distributing one without the other is useless.

## Open questions

- Why does tying embedding ↔ un-embedding weights work in practice? (Many models do it, but it is not a theoretical necessity.)
- What would adaptive or contextual tokenizers gain us?
- How can languages under-represented in the tokenization corpus be handled properly (they end up shattered into many tokens)?

## Related notes

- [[transformer-architecture.en]]
- [[attention-mechanism.en]]
- [[recurrent-networks-and-lstm.en]]
- [[world-models.en]]
- [[distillation.en]]
