---
title: "World Models"
aliases: ["world model", "model-based RL", "Dreamer", "JEPA"]
tags: [machine-learning, world-models, reinforcement-learning, model-based-rl, generative-models]
created: 2026-05-30
source: conversation with Claude
lang: en
translations:
  - "[[world-models.fr]]"
related: ["[[reinforcement-learning.en]]", "[[recurrent-networks-and-lstm.en]]", "[[embeddings-and-tokenization.en]]", "[[transformer-architecture.en]]", "[[neural-networks-overview.en]]", "[[how-neural-networks-train.en]]"]
---

# World Models

## TL;DR

> A **world model** is a **learned simulator** of an environment's dynamics: in a self-supervised way it learns the function $(s_t, a_t) \mapsto (s_{t+1}, r_t)$ — "if I take this action in this state, here is the next state and reward". Once that simulator is acquired, an agent can **train inside its own dream** rather than in the real world, which saves enormous amounts of data. It is the central building block of model-based [[reinforcement-learning.en|RL]].

## Key concepts

- **World model** — a generative network predicting the next state (and reward) conditioned on the action; it serves as a learned **simulator**.
- **Model-based vs model-free** — model-based first learns the world's dynamics and then uses them; model-free learns the policy directly, blind (see [[reinforcement-learning.en]]).
- **Latent space** — a learned, low-dimensional vector space where each point encodes the abstract essence of an observation, stripped of its redundancy.
- **V – M – C** — the canonical architecture: **V**ision (compresses the observation), **M**emory (models dynamics through time), **C**ontroller (the policy that decides).
- **Training in the dream** — learning the policy on trajectories *imagined* by the world model, without touching the real world.
- **Drift** — prediction error accumulates over a long autoregressive dream; the dream is reliable over a short horizon, fragile over a long one.

## Deep dive

### The frame shift: predicting the future, conditioned on the action

"Passive" architectures (a classifier, a [[transformer-architecture.en|Transformer]], an [[recurrent-networks-and-lstm.en|RNN]]) take an input and produce an output; the model does not act in a world that answers back. A world model is born from an [[reinforcement-learning.en|RL]] question: an agent must *act*, and there are two schools.

- **Model-free** (DQN, PPO): learns directly "in this state, which action pays best?" by trial and error. No modelling of the world — simple but **data-hungry** (millions of real interactions).
- **Model-based**: first learns a **model of the dynamics** $(s_t, a_t) \mapsto (s_{t+1}, r_t)$. That *is* a world model.

What distinguishes a world model from a plain sequence predictor is the **action as an input**: it does not predict the "natural" continuation of a sequence, but the continuation **conditioned on what the agent decides**. That is what makes it *controllable* and useful for planning or dreaming.

### The latent space (the "V")

A raw observation is huge and redundant: an image of $64 \times 64 \times 3 = 12\,288$ pixels. But the information that *matters* fits in a handful of numbers (position, velocity, curvature). An encoder learns to project the observation into a low-dimensional **latent space** (e.g. $32$ numbers):

$$ \underbrace{o_t}_{12\,288 \text{ pixels}} \;\xrightarrow{\;\text{encoder}\;}\; \underbrace{z_t}_{32 \text{ numbers}} $$

Properties: compression (throw away the superfluous, keep the degrees of freedom), semantic geometry (two similar situations → nearby points), continuity (you can interpolate). It is the same idea as text [[embeddings-and-tokenization.en|embeddings]] — an embedding *is* a latent vector — except that here you compress a continuous input (an image) and the latent is reversible (a decoder reconstructs the observation).

### How the "V" is trained: the VAE

The **V** is a **neural network** — in fact *two* ([[neural-networks-overview.en|networks]], an encoder + a decoder), trained together by gradient descent ([[how-neural-networks-train.en]]).

**The starting point — the plain autoencoder.** Two networks back to back around a bottleneck:

```
image (12,288) ─► ENCODER ─► z (32) ─► DECODER ─► reconstructed image (12,288)
```

It is trained **self-supervised** — the label *is* the input itself — to minimise the reconstruction error:

$$ \mathcal{L}_{\text{recon}} = \lVert\, o - \text{decoder}(\text{encoder}(o)) \,\rVert^2 $$

To reconstruct well through a $32$-number bottleneck, the network *must* compress the essentials into it. The gradient flows back through the decoder and then the encoder, in the usual way.

**The "V" for Variational.** A plain autoencoder is enough to compress, but its latent space is full of **holes**: draw a random $z$ and the decoder produces mush — fatal for a world model that has to *dream* plausible states. The VAE fixes this: the encoder produces not a point $z$ but the parameters of a Gaussian **distribution**, from which you sample:

$$ \text{encoder}(o) \rightarrow (\mu, \sigma), \qquad z \sim \mathcal{N}(\mu, \sigma^2) $$

The loss gains a second term — a regulariser (Kullback–Leibler divergence) pushing those Gaussians towards $\mathcal{N}(0, I)$:

$$ \mathcal{L}_{\text{VAE}} = \mathcal{L}_{\text{recon}} + \beta \cdot D_{\text{KL}}\big(\mathcal{N}(\mu,\sigma^2)\,\Vert\,\mathcal{N}(0,I)\big) $$

The two terms pull against each other (precise, scattered codes vs everything squashed around the origin). The compromise yields a latent that is **smooth, continuous and hole-free**: a randomly drawn point decodes into a plausible observation. **That is precisely what makes dreaming possible.**

**The reparameterisation trick.** Sampling $z \sim \mathcal{N}(\mu,\sigma^2)$ is not differentiable, so the gradient cannot flow back to the encoder. You take the randomness out of the path:

$$ z = \mu + \sigma \odot \varepsilon, \qquad \varepsilon \sim \mathcal{N}(0, I) $$

The noise $\varepsilon$ is drawn separately, like a fixed input; $z$ becomes a differentiable function of $\mu$ and $\sigma$, and backprop passes through normally.

### The dynamics (the "M")

The **M** models the dynamics through time. It is straightforwardly an [[recurrent-networks-and-lstm.en|RNN/LSTM]]: given the current latent, the action and the hidden state, it predicts the **next latent** and the **reward**:

$$ (z_t, a_t, h_t) \;\xrightarrow{M}\; \big(\, P(z_{t+1}),\; \hat r_t,\; h_{t+1} \,\big) $$

This is the usual recurrence scheme, but (1) the **action** is injected as an input and (2) it predicts the next **state of the world** instead of a token. The fact that $M$ also predicts $\hat r_t$ is crucial: it makes the dream **self-sufficient** in reward.

**A modern variant: the Transformer.** Predicting $z_{t+1}$ from the history of $(z, a)$ *is* sequence modelling — a [[transformer-architecture.en|Transformer]] excels at it (parallelisable training, long dependencies). That is what IRIS, TransDreamer and Decision/Trajectory Transformer do. The usual trade-off: the RNN/GRU is light and streams (hence its use in Dreamer); the Transformer costs more but models longer horizons.

### The policy (the "C") and training in the dream

The **C** is often tiny (linear): it takes $[z_t, h_t]$ and outputs the action. All world knowledge is in $M$; $C$ only has to exploit it.

**You do not "design" $C$ — you learn it.** You only set the **objective** (the reward, e.g. "survive a long time"); $C$ discovers the policy that maximises it all by itself. And it learns that **in the dream**: an episode is rolled out entirely inside $V+M$, without the real game.

```
z_t,h_t ─► C ─► a_t ─► M ─► z_{t+1}, r̂_t, h_{t+1} ─► C ─► a_{t+1} ─► M ─► ...
        (decides)     (dreams the continuation + the reward)
```

The reward does not come from the real world but from the **prediction $\hat r_t$ learned by $M$**. You sum the imagined rewards = the policy's score, which you maximise (by evolution in the 2018 paper, given how small $C$ is; by gradient in Dreamer). Thousands of imaginary episodes, for free — that is the whole data-efficiency gain.

### The full pipeline, and the "not from nothing" nuance

1. **Collection** — a random agent interacts with the real world; you record the quadruples $(o_t, a_t, o_{t+1}, r_t)$. The only contact with the real world; self-supervised (the observed future *is* the label, **no human annotation**).
2. **Train $V$** (frame by frame), then encode the whole log into sequences of latents.
3. **Train $M$** on those sequences to predict $z_{t+1}$ and $\hat r_t$.
4. **Train $C$** inside the dream generated by $V+M$.

A misconception to dissolve: this is **not** "creating a dataset out of nothing". The data cost is **moved**, not removed — you pay for real experience *once* to learn the world model, and the policy then benefits at will. And the dream can only produce what lies **inside the envelope of what was learned** (never saw a wall explode → never dreams it); errors **drift** over long horizons. Hence, in modern versions (Dreamer), a **loop**: collect → improve $V,M$ → improve $C$ → collect again with a better $C$ (which reaches new regions) → repeat.

### In use: deployment

You plug the real environment back in. **All three** networks run at every step:

```
o_t (real image) ─V─► z_t ─► C(z_t, h_t) ─► a_t ─► [act in the real world]
                         ▲                                  │
                         └── h_{t+1} ◄─ M(z_t, a_t, h_t)   o_{t+1} (real image)
```

An important nuance: in use, $M$ serves for its **memory $h_t$**, not for its prediction — you receive real frames, so there is no need to hallucinate $z_{t+1}$ any more (that was for training $C$). If $C$ had no memory ($C(z_t)$ alone), $V+C$ would suffice; but as soon as the past must be remembered, $M$ remains necessary.

### The two senses of "world model" (and the LeCun debate)

The term has two meanings, a source of confusion:

- **The broad sense** — any model that predicts the next step and encodes a dynamics of the world. In that sense, **an LLM is a world model of text**: token = state, predicting the next token = the dynamics, choosing the token = the action (RLHF literally treats the LLM as a policy). Since text is already discrete and self-labelling, the Transformer **merges $V$ and $M$** into a single network.
- **The strict sense (Yann LeCun)** — a model that predicts the *consequences of an action*, in an *abstract representation space*, *grounded in the sensory*, and *usable for planning*. By that definition, an LLM is **not** a good world model.

The "LeCun paradox" (LLMs are world models *and* a dead end) resolves this way: it is mostly a disagreement about **definition**, doubled with a substantive thesis. His criticisms of generative autoregression on text: (1) text is an impoverished projection of reality (a baby learns physics from the senses, with less data and a better model); (2) autoregressive errors **compound exponentially**; (3) reconstructing every detail (pixels, tokens) is the wrong objective. His proposal, **JEPA** (Joint Embedding Predictive Architecture, V-JEPA for video): predict in the **abstract space of representations**, **reconstructing nothing** (non-generative), keeping what is predictable and discarding what is not.

## Examples & analogies

- **The learned physics engine.** A world model is like a video game's physics engine — except that instead of being hand-coded, it was *learned* from observations. It decides nothing; it simulates. The policy $C$ is the *player* training inside that engine: it does not know the physics, it learns to win by playing thousands of imaginary games.
- **CarRacing (the founding paper, Ha & Schmidhuber 2018).** A top-down car: $o_t$ = a $64\times64$ image, $a_t$ = [steering, throttle, brake], $r_t$ = track covered. You collect random episodes, train $V$ (a VAE, $z$ in $32$ dims), then $M$ (an LSTM, $h$ in $256$ dims) on the latent sequences, then $C$ (linear, $288 \to 3$) inside the dream — never touching the real game for that last step.

## Open questions

- **Extracting a formula from the learned law.** The WM learns an empirical, black-box dynamics. Can **symbolic regression** (AI Feynman, PySR) be applied to conjecture a formula? Feasible and active, but the latent is "entangled": it must be **disentangled** for its dimensions to correspond to readable physical quantities.
- **Genie & latent actions.** How can *unsupervised* actions be learned from raw video ("which action would explain the move from frame $n$ to $n+1$?") to make a world learned without action labels controllable?
- **Sora / GAIA-1 as "world simulators".** How far does a massively trained video generator constitute a world model usable for planning?
- **JEPA vs generative autoregression.** Will LeCun's non-generative bet beat scaled autoregression, or the reverse?

## Related notes

- [[reinforcement-learning.en]]
- [[recurrent-networks-and-lstm.en]]
- [[embeddings-and-tokenization.en]]
- [[transformer-architecture.en]]
- [[neural-networks-overview.en]]
- [[how-neural-networks-train.en]]
