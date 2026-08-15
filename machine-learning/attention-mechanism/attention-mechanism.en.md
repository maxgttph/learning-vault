---
title: "The Attention Mechanism (Q, K, V, O)"
aliases: ["attention", "self-attention", "multi-head attention"]
tags: [machine-learning, attention, transformer, llm]
created: 2026-05-26
source: conversation with Claude
lang: en
translations:
  - "[[attention-mechanism.fr]]"
related: ["[[transformer-architecture.en]]", "[[embeddings-and-tokenization.en]]", "[[llm-inference-optimization.en]]", "[[how-neural-networks-train.en]]", "[[normalization.en]]", "[[bias.en]]", "[[recurrent-networks-and-lstm.en]]", "[[mixture-of-experts.en]]"]
---

# The Attention Mechanism (Q, K, V, O)

## TL;DR

> Attention is **differentiable routing of information between tokens**. Three learned matrices **W_Q, W_K, W_V** project each token into three spaces (question / label / content); a fourth, **W_O**, post-processes the result. The crucial point: these matrices are **mathematically identical to start with** — their distinct roles emerge purely from their **position in the formula**, and from the gradient pressure that specialises them during training.

## Key concepts

### Learned matrices (parameters, frozen after training)

- **W_Q, W_K, W_V** — three $d_{\text{model}} \times d_k$ matrices projecting each embedding into three different spaces. Identical by nature (same shape, same initialisation, same update algorithm). Their distinct roles come from their **position in the formula**, not from any intrinsic property.
- **W_O** — a fourth matrix (typically $d_{\text{model}} \times d_{\text{model}}$). It plays in a different league: it takes **no** part in the routing, only in the post-processing.

### Vectors computed on the fly (transient activations)

- **Q, K, V** — for each token, the vectors obtained by applying the $W_*$ matrices to its embedding. $Q$ = "what I am looking for", $K$ = "my public label", $V$ = "what I pass on".
- **Scores and A** — the $n \times n$ matrix of similarities between each pair of tokens, then its normalisation into a distribution.

### Additional concepts

- **Causal mask** — forbids tokens from looking at the future. Essential to autoregressive prediction.
- **Multi-head attention** — several attentions in parallel (typically 8 to 64 heads), each capturing a different type of relation.

## Deep dive

### The terminological trap: matrices vs vectors

"Q, K, V" refers to two different things depending on context:

| Notation | What it is | Status |
|---|---|---|
| $W_Q$, $W_K$, $W_V$, $W_O$ | Learned weight matrices | Parameters, frozen after training |
| $Q$, $K$, $V$ | Vectors computed on the fly for each token | Transient activations, recomputed at every inference |

The $W_*$ live in the `.safetensors` file. The $Q, K, V$ exist only at the instant the token passes through the layer. Remember: **the matrices are the parameters, the vectors are the activations**.

### The library analogy

You arrive with a question in mind (Q). Each book has a label on its spine (K) and content inside (V). You compare your Q with each K and walk away with a **weighted blend** of the books' content V — a lot from the one whose label matches, little (or none) from the others.

Attention does exactly that, except that:
- Each token is simultaneously a reader AND a book.
- The comparison is differentiable (dot product + softmax).
- Everything is learned by gradient descent.

### The mathematical recipe, step by step

For a sequence $X$ (each row = one token):

**Step 1 — project** (3 matrices in parallel, one Q/K/V vector per token):

$$Q = X\, W_Q \qquad K = X\, W_K \qquad V = X\, W_V$$

**Step 2 — similarity scores** ($n \times n$ matrix):

$$\text{scores} = \frac{Q\, K^\top}{\sqrt{d_k}}$$

where $\text{scores}_{ij}$ measures "how much token $i$ is interested in token $j$".

**Step 3 — causal mask + softmax** (each row becomes a distribution):

$$A = \text{softmax}(\text{scores} + \text{mask})$$

**Step 4 — weighted blend of contents** (each row $i$ = a combination of the $V_j$ weighted by $A_{ij}$):

$$\text{mixed} = A\, V$$

**Step 5 — final projection** (post-processing, back to $d_{\text{model}}$):

$$\text{out} = \text{mixed} \cdot W_O$$

Compactly, this is the canonical formula from *Attention Is All You Need*:

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{Q\, K^\top}{\sqrt{d_k}}\right) V$$

### Why the roles differ if the matrices are identical

This is the deep question about the mechanism. If nothing intrinsically distinguishes W_Q from W_K from W_V (same shape, same initialisation, same update), why do they end up with such different roles?

The answer: **two complementary mechanisms**.

**1. Structural asymmetry (position in the formula)**

- **W_Q vs W_K**: their outputs meet in $Q\, K^\top$. But Q is on the left, K transposed on the right. Consequently:

  $$\text{score}_{ij} = Q_i \cdot K_j$$

  This product is **not symmetric**: $\text{score}_{ij} \neq \text{score}_{ji}$ in general. Hence the "who listens / who is listened to" distinction — encoded in the **left/right** position in the product, not in the matrices.

- **W_V vs W_Q/W_K**: the output of W_V appears **only after** the softmax, multiplied by A. It takes no part at all in computing the scores. That position is what makes it "the content passed on" rather than "the routing filter".

**2. Gradient pressure (specialisation forced by training)**

During training, each matrix receives its gradient through the path where its output is used:

- $W_V$ receives $\text{Loss} \to \text{mixed} = A V \to V \to W_V$.
  The gradient's message: *"change yourself so that the blended content is useful at the output."*

- $W_Q$ receives $\text{Loss} \to \text{mixed} \to A \to \text{softmax} \to \text{scores} \to Q \to W_Q$.
  The gradient's message: *"change yourself so that the scores produce better routing."*

- $W_K$: same as $W_Q$, through the symmetric path on the K side.

The matrices start out identical (Gaussian noise). But because the gradient pressure on each is different, **they diverge through forced specialisation**. On day 0, indistinguishable. On day N, experts in their structural role.

**Thought experiment**: if you swapped the roles W_V ↔ W_Q in the formula before training (without changing the initial matrices), the system would work just the same. In the end, the matrix we would have called W_V would look like what we used to call W_Q, and vice versa. **What gets learned depends entirely on where the matrix is plugged in.**

### Why W_O is not on the same footing as Q/K/V

Technically, W_O is a learned matrix like the other three. But conceptually:

- $W_Q, W_K, W_V$ **take part in the attention mechanism proper** — they decide who listens to whom and with what content.
- $W_O$ comes **after** attention. If you replaced $W_O$ with the identity, attention would still work (tokens would listen to each other correctly), the result would simply come out in a non-recombined space. Whereas without $W_V$ there would literally be nothing to pass on. Without $W_Q$ or $W_K$, the routing would be broken.

A more honest partition:
- **Routing phase**: W_Q, W_K, W_V
- **Integration phase**: W_O

W_O mainly plays two roles:
1. **Stitching the heads back together** in multi-head — the concatenated outputs of the N heads are mixed together by W_O.
2. **Reprojecting** into the $d_{\text{model}}$ dimension of the residual stream.

The "Q, K, V, O" convention is kept because all 4 are structurally similar in code, but it helps to know that W_O plays for a different team.

### Attention ≠ statistical co-occurrence

A classic trap: thinking attention learns "which words often appear together". Wrong.

- **Statistical co-occurrence** is captured by the [[embeddings-and-tokenization.en|embeddings]] (cf. word2vec).
- **Attention** learns **contextual routing**: "given each token's state (already contextualised), which other tokens hold the information I need in order to transform myself usefully?"

Example: in "The cat that Marie adopted yesterday is sleeping", when the model processes "sleeping", it needs to recover the **syntactic subject** ("cat", seven words earlier). That is a grammatical relation, not a statistical correlation.

### Multi-head: several specialists in parallel

For Llama 3 70B: $d_{\text{model}} = 8192$, 64 heads of dimension $d_k = 128$. Each head has its own W_Q, W_K, W_V, and learns a different type of relation: anaphoric references, subject-verb syntax, negation, contrast, and so on. The heads' outputs are concatenated, then W_O recombines them.

One attention layer therefore contains 4 matrices of $8192 \times 8192 \approx 270\text{M}$ parameters. Across 80 layers, $\approx 21$ billion for attention alone.

## Examples & analogies

- **A universal library**: each token is simultaneously a reader (Q) and a book (K, V). Everyone reads everyone in a single parallel step.
- **Three identical workers.** Hire three workers with the same training, post one at reception, one at sorting, the third at dispatch. A year later they are radically different experts — not by nature, but because their position in the production chain shaped their expertise. That is exactly what happens to W_Q, W_K, W_V during training.
- **A neuroscience analogy.** A visual cortex neuron is nothing intrinsically "visual" — it became so because it is wired downstream of the retina. Mriganka Sur's experiments (in the 1990s) showed that rewiring that neuron to the inner ear makes it auditory. Meaning emerges from the structure of the graph, not from intrinsic content.

## Open questions

- How can we interpret mechanistically what each head has learned? (mechanistic interpretability)
- Which alternatives to quadratic attention are viable? (linear attention, sliding window, sparse, state-space)
- Why does multi-head work better than one big head? Regularisation? Diversity of learned routings?
- What happens if you initialise $W_Q = W_K = W_V$ to **exactly identical** values (not merely the same distribution)? Is the symmetry broken by SGD noise at the first step?

## Related notes

- [[transformer-architecture.en]]
- [[embeddings-and-tokenization.en]]
- [[llm-inference-optimization.en]]
- [[how-neural-networks-train.en]]
- [[normalization.en]]
- [[bias.en]]
- [[recurrent-networks-and-lstm.en]]
- [[mixture-of-experts.en]]
