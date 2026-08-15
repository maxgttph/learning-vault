---
title: "Emulsions"
aliases: [Emulsifiers, Emulsified sauces]
tags: [food-chemistry, emulsion, surfactant, physical-chemistry, cooking]
created: 2026-08-10
source: conversation with Claude
lang: en
translations:
  - "[[emulsions.fr]]"
related:
  - "[[proteins-in-cooking.en]]"
  - "[[thickeners-and-starch.en]]"
  - "[[fats-heat-and-aroma.en]]"
---

# Emulsions

## TL;DR

> An emulsion is a forced dispersion of fat droplets in water (or the reverse), held together by amphiphilic molecules that line the interface. Almost all of them are **thermodynamically unstable**: they survive only because the emulsifier raises a **kinetic barrier** against droplets merging. The question is never "will it break?" but "how long will it take?".

## Key concepts

- **Emulsion** — a dispersion of one liquid as fine droplets in another, immiscible one. "Oil-in-water" (mayonnaise, coconut milk) or "water-in-oil" (butter).
- **Hydrophobic effect** — oil and water do not repel each other: it is water that overwhelmingly prefers to bond with itself. A largely **entropic** phenomenon.
- **Interfacial tension $\gamma$** — the energy cost per unit area of the oil-water boundary. In $\text{J}\cdot\text{m}^{-2}$ (or $\text{N}\cdot\text{m}^{-1}$).
- **Amphiphile / surfactant** — a molecule with a hydrophilic head and hydrophobic tail(s), which adsorbs at the interface and lowers $\gamma$.
- **Coalescence** — the irreversible merging of two droplets whose protective film has given way. This is the real "breaking".
- **Creaming** — droplets rising (or settling) through a density difference, without merging. **Reversible.**

## Deep dive

### Why water and oil ignore each other

Water is **polar**: oxygen, far more electronegative than hydrogen, pulls the electrons in, creating a partial negative charge on $\text{O}$ and positive charges on the $\text{H}$s. Because the molecule is bent, those charges do not cancel — water carries a dipole and weaves a dense network of hydrogen bonds.

Oil is **non-polar**: long hydrocarbon chains in which the $\text{C}\!-\!\text{H}$ bonds are electrically near-symmetric. No dipole, no hydrogen bonding — only weak London forces.

The counter-intuitive point: there is no oil-water "repulsion". What dominates is the **hydrophobic effect**, a matter of entropy. Around a drop of oil, water molecules can no longer form hydrogen bonds in every direction; they reorganise into a more ordered cage, hence of low entropy. The system minimises that penalty by reducing the contact area as much as possible. **Drops merge spontaneously because less surface = less constrained water.**

### Interfacial tension, and why whisking costs energy

The energy cost of creating interface is written:

$$\Delta G = \gamma \cdot \Delta A$$

Making an emulsion means tearing the oil into microscopic droplets, hence **increasing $\Delta A$ enormously**: $1\ \text{mL}$ of oil broken into $1\ \mu\text{m}$ droplets develops several $\text{m}^2$ of interface. Hence the mechanical energy needed (the whisk), and hence the system's spontaneous urge to go back — the separated state has less interface, so a lower $G$.

The emulsifier acts on the other term: by inserting itself at the interface, it **lowers $\gamma$**. Creating interface becomes much cheaper, and undoing it much less urgent.

### The emulsifier: lowering $\gamma$ and raising a barrier

An amphiphilic molecule finds at the interface the one position that satisfies both of its ends: tail plunged into the oil, head in the water. By lining the droplet it does two things — lower $\gamma$, and physically prevent merging. Two stabilisation mechanisms:

- **Electrostatic repulsion** — if the head carries a charge (carboxylate $\text{–COO}^-$, phosphate), every droplet carries the same charge and they repel one another. A double layer of ions forms around each; their overlap creates a repulsive barrier. *Corollary: salt, acid and calcium destabilise the emulsion by screening those charges.*
- **Steric hindrance** — large molecules (proteins, polysaccharides) form a physical mattress. Two droplets do not merge because there is literally matter in between.

The best emulsifiers combine both. Egg yolk **lecithin** is the archetype: a phospholipid with a glycerol backbone, two fatty-acid tails well anchored in the oil, and a **zwitterionic** phosphate-choline head (a $+$ charge and a $-$ charge). Two tails for anchoring, a charged head for repulsion.

Not all emulsifiers are proteins — that is the classic mistake. Three classes:

| Class | Examples | Mechanism | Temperature |
|---|---|---|---|
| Small amphiphilic molecules | Yolk lecithin, mono-/diglycerides | Fast adsorption, big drop in $\gamma$ | Effective **cold** |
| Proteins | Caseins, yolk lipoproteins, coconut, peanut | Viscoelastic film, mostly **steric** | Depends on type, see [[proteins-in-cooking.en]] |
| Polysaccharides (gums) | Mustard mucilage, gum arabic, xanthan | Hindrance + thickening of the medium | Cold, stable to heat |

Mustard therefore emulsifies not through its proteins but mainly through its **mucilages** — polysaccharides from the seed coat. It is a weak emulsifier: hence the structural fragility of a vinaigrette.

### The central idea: unstable, yet long-lasting

Almost all culinary emulsions are **thermodynamically unstable**. The lowest-energy state is always "oil on one side, water on the other". What keeps them together is not thermodynamics but **kinetics**: the emulsifier raises a barrier so high that merging becomes extremely slow.

A mayonnaise holds for hours not because it is stable, but because the barrier raised by lecithin is enormous. A vinaigrette breaks in three minutes because mustard's barrier is low. *(A rare exception: **microemulsions**, with nanometre droplets, genuinely stable and spontaneously forming.)*

### The four deaths of an emulsion

```
dispersed state
     │
     ├─► CREAMING / SEDIMENTATION  droplets intact, sorted by density  REVERSIBLE
     │
     ├─► FLOCCULATION              clusters of droplets, films intact  REVERSIBLE
     │
     ├─► COALESCENCE               films broken, 2 droplets → 1        IRREVERSIBLE
     │
     └─► OSTWALD RIPENING          big ones grow at the expense of
                                   small ones (Laplace pressure)       IRREVERSIBLE
```

Ostwald ripening is the subtlest: a droplet's internal pressure follows Laplace's law, $\Delta P = 2\gamma / r$. Small droplets, at high internal pressure, see their oil molecules slowly diffuse through the aqueous phase towards the big ones. Slow, but with no way back.

### Three practical cases, three diagnoses

**Vinaigrette that collapses.** An emulsion born fragile: mustard cannot cover all the droplets. When the whisking stops, the air foam collapses (film drainage, bubbles bursting), then the emulsion creams and coalesces. The "clustered little bubbles" you see are oil sticking back together; the "juice" is the denser aqueous phase running underneath. **A construction problem.**

**Mafé, coconut curry.** Here the emulsion pre-exists and is robust: coconut milk is an oil-in-water emulsion stabilised at the factory by coconut proteins; peanut butter is a suspension of ground particles in peanut oil. The culprit is **thermal** — denaturation and then aggregation of the proteins that lined the droplets (the film falls off), evaporation of water which unbalances the fat/water ratio, thinning of the aqueous phase which speeds up collisions, and turbulent boiling which mechanically tears the films. **A preservation-under-heat problem.** See [[proteins-in-cooking.en]].

**Mayonnaise that turns grainy.** No heat, so no protein coagulation. Either droplets coalesced into clusters (oil added too fast: locally more oil than the lecithin can line), or **triglyceride crystals** if the oil or the mayonnaise got cold. Either way it is physical.

> **The reversibility test is the best diagnosis.** If whisking brings it back, it was physics (fat). If it stays lumpy whatever you do, it was chemistry (coagulated proteins).

### Building, and rescuing

For an emulsion to hold: more emulsifier; oil poured in a **thin stream** while whisking continuously (otherwise you exceed the encapsulation capacity); the aqueous phase / emulsifier / oil ratio respected; and if it must hold for a long time, a strong emulsifier (egg yolk) or a thickener that freezes the droplets in place (see [[thickeners-and-starch.en]]).

To rescue: re-whisking is often enough if you are only at the creaming stage. If it has truly split, use the **seed method** — in a clean bowl, a spoonful of fresh mustard (or warm water, or a yolk), and pour the broken sauce into it in a thin stream while whisking. You rebuild the emulsion around a base of fresh emulsifier.

## Examples & analogies

- **The whisk pays a surface bill.** Emulsifying means buying interface with mechanical energy, at a unit price of $\gamma$. The emulsifier is the trade discount: it lowers the price per square metre, and posts a guard in front of every droplet.
- **Vinaigrette and mayonnaise are the same object on two timescales.** Nothing distinguishes them conceptually — only the height of the kinetic barrier: three minutes versus a whole evening.
- **Separation is sometimes the technique, not the failure.** In Thai cooking you deliberately boil coconut cream hard to break it ("crack the coconut cream") and fry the curry paste in the coconut oil released. A well-made mafé almost always shows its orange oil slick. The target is not "zero separation" but **controlled separation**.

## Open questions

- **HLB** (Hydrophilic-Lipophilic Balance): the scale that ranks emulsifiers by the balance between their hydrophilic and lipophilic parts — and predicts whether you get an oil-in-water or water-in-oil emulsion.
- Why are some emulsions **inverse** (water-in-oil: butter, margarine)? Bancroft's rule.
- Emulsions stabilised by **solid particles** (Pickering emulsions) — neither surfactant nor protein.

## Related notes

- [[proteins-in-cooking.en]]
- [[thickeners-and-starch.en]]
- [[fats-heat-and-aroma.en]]
