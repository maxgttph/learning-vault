---
title: "How a Neuron Works"
aliases: []
tags: [neuroscience, neuron, electrophysiology, action-potential]
created: 2026-06-20
source: conversation with Claude
related: ["[[cortical-hyperexcitability]]", "[[migraine-pathophysiology]]"]
---

# How a Neuron Works

## TL;DR

> A neuron is a cell that receives, integrates and transmits electrical signals. It works by maintaining an ion gradient across its membrane (a charged "battery"), discharging an all-or-nothing **action potential** when stimulation crosses a threshold, and passing the message chemically to the next neuron at a **synapse**. An energy-hungry pump constantly recharges the battery.

## Key concepts

- **Resting potential** — the neuron's charged-but-idle state: inside negative (~$-70$ mV) relative to outside.
- **Ion channels** — selective protein tunnels for a single ion (Na⁺, K⁺, Ca²⁺); either *voltage-gated* (open on voltage change) or *ligand-gated* (open when a neurotransmitter binds).
- **Threshold** — the membrane voltage (~$-55$ mV) above which firing becomes inevitable. The single most important idea for excitability.
- **Action potential** — the all-or-nothing electrical discharge that travels down the axon.
- **Synapse** — the gap where the signal is handed to the next neuron, chemically.
- **Neurotransmitters** — chemical messengers; **glutamate** excites, **GABA** inhibits.
- **Na⁺/K⁺ pump** — the ATP-consuming "cleaner" that restores the gradient after firing.

## Deep dive

### Anatomy

A neuron has three functional parts: **dendrites** (the antennae receiving signals from other neurons), the **cell body / soma** (which integrates everything), and the **axon** (the cable carrying the output to its terminals and onward to the next neurons).

### Resting potential — a charged battery

At rest the neuron is **polarised**: the inside sits at roughly $-70$ mV relative to the outside. This is sustained by an uneven distribution of ions across the membrane — lots of **sodium ($\text{Na}^+$)** and **calcium ($\text{Ca}^{2+}$)** outside, lots of **potassium ($\text{K}^+$)** inside. Like a charged battery, it is primed to discharge.

### Ion channels — selective doors

The membrane is studded with **ion channels**, protein tunnels each passing one specific ion. *Voltage-gated* channels open when the membrane voltage shifts; *ligand-gated* channels open when a neurotransmitter binds.

### The action potential — all-or-nothing

1. Incoming excitatory signals depolarise the membrane slightly (make it less negative).
2. If it reaches **threshold** (~$-55$ mV), voltage-gated $\text{Na}^+$ channels open en masse → $\text{Na}^+$ rushes in → the inside becomes positive. This is the **action potential**: a fast, **all-or-nothing** spike — it either fires fully or not at all.
3. $\text{K}^+$ channels then open → $\text{K}^+$ flows out → the membrane returns to negative (**repolarisation**).
4. The spike propagates along the axon to the terminals.

The takeaway: anything that pushes the membrane closer to threshold makes the neuron **easier to fire** — i.e. more excitable.

### The synapse — handing over the message

At the axon terminal the electrical signal must cross a tiny gap (the **synapse**):

1. The action potential opens $\text{Ca}^{2+}$ channels → calcium enters.
2. Calcium triggers release of **neurotransmitters** into the synapse.
3. They bind receptors on the next neuron:
   - **Glutamate** → opens channels admitting $\text{Na}^+/\text{Ca}^{2+}$ → **excites** (pushes toward threshold).
   - **GABA** → opens channels admitting $\text{Cl}^-$ (or releasing $\text{K}^+$) → **inhibits** (pushes away from threshold).

Each neuron continuously sums thousands of excitatory (glutamate) and inhibitory (GABA) inputs; it fires only if the sum crosses threshold.

### The Na⁺/K⁺ pump — the energy-hungry cleaner

After firing, ions are misplaced ($\text{Na}^+$ has entered, $\text{K}^+$ has left). The **$\text{Na}^+/\text{K}^+$ pump** restores order — ejecting $\text{Na}^+$, pulling $\text{K}^+$ back in — by burning **ATP** (cellular energy from mitochondria). It is the system's weak point: when the energy supply falters, the pump is overwhelmed and the gradient degrades.

## Examples & analogies

- **Charged battery**: the resting potential stores energy ready to release; the pump keeps it charged.
- **Trigger with a set point**: firing is all-or-nothing past the threshold — like a gun's trigger, not a dimmer. Lowering the threshold is what "hair-trigger" excitability means.

## Open questions

- How myelin and saltatory conduction speed propagation along the axon.
- How glial cells (astrocytes) buffer extracellular $\text{K}^+$ and glutamate.

## Related notes

- [[cortical-hyperexcitability]]
- [[migraine-pathophysiology]]
