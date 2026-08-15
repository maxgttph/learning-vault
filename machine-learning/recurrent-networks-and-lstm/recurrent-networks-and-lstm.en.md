---
title: "Recurrent Networks (RNN) and LSTM"
aliases: ["RNN", "LSTM", "GRU", "recurrent networks"]
tags: [machine-learning, rnn, lstm, sequences, gradient]
created: 2026-05-29
source: conversation with Claude
lang: en
translations:
  - "[[recurrent-networks-and-lstm.fr]]"
related: ["[[neural-networks-overview.en]]", "[[how-neural-networks-train.en]]", "[[transformer-architecture.en]]", "[[attention-mechanism.en]]", "[[embeddings-and-tokenization.en]]", "[[llm-inference-optimization.en]]", "[[reinforcement-learning.en]]", "[[world-models.en]]"]
---

# Recurrent Networks (RNN) and LSTM

## TL;DR

> An **RNN** processes a sequence element by element while carrying a hidden state $h_t$ (a memory). Propagating the gradient through time amounts to **multiplying** a long chain of terms, which makes it vanish → short memory. The **LSTM** adds a memory cell updated by **addition** and controlled by **learned gates**, giving the gradient a motorway on which it is preserved — which makes long dependencies learnable.

## Key concepts

- **Sequence** — an ordered series of elements where order matters (words, time series, audio, video frames).
- **Hidden state $h_t$** — a vector summarising "everything read up to step $t$"; the RNN's raw output at each step.
- **Recurrence** — the same operation and the **same set of weights** reapplied at every time step, hence the need for $h_{t-1}$ to compute $h_t$ (sequential by construction).
- **BPTT (backpropagation through time)** — backpropagation over the RNN "unrolled" through time; the gradient climbs back up the chain of $h_t$.
- **Vanishing / exploding gradient** — the product of a long chain of derivatives tending to 0 (memory lost) or exploding (divergence).
- **Memory cell $C_t$** (LSTM) — a "conveyor belt" of information modified by small touches rather than rewritten.
- **Gates** — sigmoids emitting values in $[0,1]$; they modulate what is forgotten, written and read.
- **GRU** — a 2-gate variant, lighter than the LSTM, often just as effective.

## Deep dive

### What an RNN takes as input and how it "processes step by step"

The input is a **list of embeddings**: the sentence is tokenized, each token becomes a vector (see [[embeddings-and-tokenization.en]]). Unlike a Transformer, the RNN does not see all those vectors at once: it **consumes them one by one**, in order, updating its hidden state.

$$h_t = \tanh(W_x\, x_t + W_h\, h_{t-1} + b)$$

To compute $h_2$ you need $h_1$: there is a dependency, so parallelising across the sequence is impossible. That is the **architectural** difference (not an encoding one) from the Transformer: the embedding is the shared input to both; it is the recurrent loop vs attention that imposes sequential vs parallel. Transformers were invented precisely to escape this sequential slowness.

### The output depends on the task

The RNN/LSTM produces a vector $h_t$ **at every step**. That is the raw output; an **output layer** added on top transforms it according to use:

- **Many-to-one** (sentiment, text classification): keep only the last $h_T$ (a global summary) → one prediction per sentence.
- **Aligned many-to-many** (part-of-speech tagging): one prediction on each $h_t$ → one tag per word.
- **Generation / next-token**: on each $h_t$, a softmax over the vocabulary predicts the next word, fed back in at the following step.
- **Seq2seq** (translation): one RNN encodes the whole source into its final state, a second decodes word by word.

### Why the simple RNN fails: the gradient through time

At training time the loop is unrolled: an RNN over 50 words becomes, from the gradient's point of view, a **50-level** computation reusing the same $W_h$ (these are not 50 physically distinct layers — it is **one** layer applied 50 times; the depth is *temporal*, not spatial). The backward pass climbs the chain of $h_t$ via the chain rule (see [[how-neural-networks-train.en]]):

$$\frac{\partial L}{\partial h_1} = \frac{\partial L}{\partial h_T} \cdot \frac{\partial h_T}{\partial h_{T-1}} \cdots \frac{\partial h_2}{\partial h_1}$$

Each factor involves $W_h$ and $\tanh' \le 1$. Multiplying ~50 terms $\le 1$ melts the product towards 0: the gradient **vanishes**, and the start of the sequence is forgotten. The simple RNN retains only ~10–20 steps. Its memory is "short-term". The name **Long Short-Term Memory** announces the programme: a short-term memory **that lasts**.

> Computing the $h_t$ (forward pass) is identical at training and at inference. The difference: at inference you stop after the forward pass; at training you follow with the backward pass, which flows **through the $h_t$**. So the $h_t$ are not "an inference thing" — they are the very medium of the gradient.

### The LSTM: separating memory from output

The LSTM carries **two** vectors through time: the memory cell $C_t$ (the conveyor belt, barely modified) and the hidden state $h_t$ (what is exposed, derived from $C_t$). Three gates ($\sigma$ → values in $[0,1]$, multiplied component-wise):

- **Forget** $f_t = \sigma(W_f\,[h_{t-1}, x_t] + b_f)$ — what to keep from the old memory.
- **Input** $i_t = \sigma(W_i\,[h_{t-1}, x_t] + b_i)$ — what new to write.
- **Output** $o_t = \sigma(W_o\,[h_{t-1}, x_t] + b_o)$ — what to read from the memory.

The update — the key equation is an **addition**, not a matrix rewrite:

$$C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t, \qquad \tilde{C}_t = \tanh(W_C\,[h_{t-1}, x_t] + b_C)$$
$$h_t = o_t \odot \tanh(C_t)$$

```
memory   C₀ ──→ C₁ ──→ C₂ ──→ C₃     ← conveyor belt (controlled addition)
                 │       │       │
output          h₁      h₂      h₃     ← read, filtered by o_t
```

### Why this solves the vanishing gradient

Because of the additive form, the gradient climbing back through the memory is essentially:

$$\frac{\partial C_t}{\partial C_{t-1}} \approx f_t$$

$f_t$ is **learned and contextual**. If the information matters, the network learns $f_t \approx 1$ and then $1 \times 1 \times \cdots \approx 1$: the gradient crosses hundreds of steps without vanishing (the **"constant error carousel"**). When something must be forgotten, $f_t \approx 0$ deliberately cuts it off. You replace a **multiplication you suffer** (which destroys the gradient) with an **addition controlled by learned gates** (which preserves it on demand). This is exactly the idea behind *skip connections* (ResNet) and the residual stream of [[transformer-architecture.en|Transformers]]: giving the gradient an additive path.

Cost: ~4× the parameters of a simple RNN (3 gates + the candidate $\tilde{C}_t$), hence the **GRU**, which does almost the same with 2 gates.

### Size of $h$ ≠ sequence length

Two independent dimensions, often confused:

- **Size of $h$** (hidden size, e.g. 128) — the *width* of the memory, a fixed hyperparameter. Every $h_t$ has that size, whatever the sentence. (The input $x_t$, e.g. a 300-dim embedding, has its own, distinct size.)
- **Sequence length** — the *number of iterations* of the loop. The RNN has **no hard limit**: it loops as many times as there are words.

The only limit is **soft**: memory of the beginning fades (vanishing gradient) — the LSTM pushes this back without eliminating it. The **hard** length limit is a feature of **Transformers** (a fixed context window), not of RNNs — a fundamental trade-off between the two families.

### An overview of RNNs

**By cell type:**

| Cell | Distinctive feature | Use |
|---|---|---|
| Simple RNN (Elman) | 1 recurrent $\tanh$ layer; vanishing gradient | teaching; rare in production |
| Jordan | recurrence from the output, not the hidden state | historical |
| **LSTM** | memory cell + 3 gates | ★ heavily used |
| **GRU** | 2 gates, lighter | ★ heavily used |
| Peephole LSTM | the gates see $C_t$ | niche (precise timing) |

**By topology:**

| Architecture | Distinctive feature | Domains |
|---|---|---|
| **Bidirectional** (Bi-LSTM/GRU) | reads the sequence both ways | ★ NER, POS tagging, speech recognition |
| Deep / Stacked | several recurrent layers stacked (spatial + temporal depth) | complex tasks |
| **Encoder-Decoder (Seq2Seq)** | one RNN encodes, another decodes | ★ translation, summarisation |
| Seq2Seq + Attention | the decoder looks at every encoder state | precursor of the Transformer (Bahdanau 2014) |
| Recursive NN (TreeRNN) | tree structure | syntactic parsing |
| Echo State / Reservoir | fixed random recurrent weights, only the output is learned | time series, research |
| Neural Turing Machine / DNC | RNN + addressable external memory | algorithmic reasoning |

**The most used**: LSTM and GRU (the workhorses), Bi-LSTM (long the NLP standard before 2018), Seq2Seq + attention.

### RNN vs Transformer: the head-to-head

The real difference is not "attention added on" — it is an opposite choice about **how to process a sequence**, from which everything else follows.

| Criterion | RNN / LSTM | [[transformer-architecture.en\|Transformer]] |
|---|---|---|
| Processing | **sequential** (one token after another) | **parallel** (all tokens at once) |
| Memory of the past | 1 vector $h_t$ of **fixed size** (a bottleneck) | **direct access to all** tokens via [[attention-mechanism.en\|attention]] |
| Distance between 2 related words | = their **distance in the sentence** (signal diluted step by step) | **always 1 attention hop**, whatever the distance |
| Compute cost | $O(n)$ but **serial** (not parallelisable) | $O(n^2)$ but **parallelisable** in one block |
| Word order | **implicit** (given by the loop's traversal) | **explicit**: a positional encoding must be injected |
| Sequence length | no **hard** limit (loops as long as needed), a **soft** limit (forgetting) | a **hard** limit (fixed context window) |

The key, counter-intuitive point: the Transformer does **more** computation ($O(n^2)$ vs $O(n)$), but it wins because that computation is **parallelisable**. You trade "little computation but in single file" (which GPUs hate) for "lots of computation but all at once" (which GPUs love). The RNN's $h_t \leftarrow h_{t-1}$ dependency chain is precisely what forbids that parallelisation.

> **An inference-time nuance**: the Transformer's parallelism advantage exists mainly at **training** time (the whole sequence is known in advance). At token-by-token **generation** it becomes sequential again — hence the importance of the KV cache (see [[llm-inference-optimization.en]]).

### Putting it in perspective

Since 2017, [[transformer-architecture.en|Transformers]] have largely supplanted RNNs in NLP (parallelisable, better long dependencies). RNNs remain relevant for **real-time streaming**, **embedded/low-compute** settings and modest **time series**. Recent **state-space** models (Mamba, S4) rehabilitate the recurrent idea with better properties.

## Examples & analogies

- **An assembly line with a central conveyor belt** ($C_t$): at each station, one worker removes things from the belt (forget gate), another puts things on (input gate), a third reads aloud for the next station without necessarily removing anything (output gate). An important part can stay intact across hundreds of stations — where a simple RNN rebuilds everything at each station and loses the original.
- **Long-distance agreement**: "the **cat** that I saw yesterday in the garden… **sleeps**". Capturing that subject-verb agreement across many words is precisely what an LSTM makes learnable and a simple RNN does not.

## Open questions

- A step-by-step account of truncated BPTT, used in practice for long sequences.
- GRU vs LSTM: when does one genuinely beat the other empirically?
- How do state-space models (Mamba, S4) combine recurrence with parallel training?

## Related notes

- [[neural-networks-overview.en]]
- [[how-neural-networks-train.en]]
- [[transformer-architecture.en]]
- [[attention-mechanism.en]]
- [[embeddings-and-tokenization.en]]
- [[llm-inference-optimization.en]]
- [[reinforcement-learning.en]]
- [[world-models.en]]
