---
title: "Stellar Nuclear Fusion and the Origin of Matter"
aliases: [Proton-proton chain, Deuterium, Mass defect, Big Bang nucleosynthesis, CNO cycle]
tags: [astronomy, nuclear-physics, cosmology, sun, nucleosynthesis]
created: 2026-08-11
source: conversation with Claude
lang: en
translations:
  - "[[nuclear-fusion-and-origin-of-matter.fr]]"
related:
  - "[[structure-of-the-sun.en]]"
  - "[[spectroscopy-and-black-body.en]]"
  - "[[solar-wind.en]]"
  - "[[general-atmospheric-circulation.en]]"
---

# Stellar Nuclear Fusion and the Origin of Matter

## TL;DR

> Fusion does not ignite a star — **gravity** heats it, and fusion **stops** the collapse. No matter is ejected by fusion: four protons bind into a helium nucleus $0.7\%$ lighter, and that missing mass becomes energy. The hydrogen feeding all this dates from the universe's first microsecond.

## Key concepts

- **Proton-proton chain** — the dominant fusion route in the Sun ($\approx 99\%$ of the energy): $4\,{}^1\text{H} \rightarrow {}^4\text{He}$.
- **Deuterium** — an isotope of hydrogen (1 proton + 1 neutron); the only bound two-nucleon state possible.
- **Mass defect** — a bound system weighs *less* than the sum of its constituents; the difference is the binding energy.
- **The weak-interaction bottleneck** — the first step ($p+p$) requires transmuting a proton into a neutron, which slows it by an astronomical factor.
- **Self-regulating hydrostatic equilibrium** — the thermostat that keeps a star stable for billions of years.
- **Photon random walk** — $\approx 10^5$ years for a gamma ray from the core to get out, fragmented into visible photons.
- **Big Bang nucleosynthesis (BBN)** — the first three minutes, producing $75\%$ H / $25\%$ He and nothing heavier.
- **The deuterium bottleneck** — the block that delays primordial nucleosynthesis until $t \approx 3$ min.

## Deep dive

### Deuterium

It is an **isotope of hydrogen**: same element, same chemistry, heavier nucleus.

| | Nucleus | Terrestrial abundance | Stability |
|---|---|---|---|
| Protium (${}^1\text{H}$) | 1 proton | $99.98\%$ | stable |
| Deuterium (${}^2\text{H}$ or D) | 1 proton $+$ 1 neutron | $0.0156\%$ | stable |
| Tritium (${}^3\text{H}$) | 1 proton $+$ 2 neutrons | traces | radioactive, $T_{1/2} = 12.3$ years |

On Earth, roughly **one deuterium atom per $6\,400$ hydrogen atoms**, essentially all in the oceans.

**Its decisive nuclear property.** The deuteron is bound by only $2.22$ MeV — extraordinarily little. And above all, **it is the only bound two-nucleon state possible**: there is no "diproton" and no "dineutron". That absence forces stellar fusion to go through the weak interaction, which slows it by an astronomical factor — and it is that slowness that gives the Sun ten billion years instead of a few million. A universe where the diproton were bound would have stars flaring up and burning out in the blink of an eye; there would be nobody around to watch.

**Its discovery** loops back nicely to spectroscopy. Urey, in 1931–1932, distilled liquid hydrogen and then looked at its spectrum. A more massive nucleus shifts the electronic levels slightly (reduced-mass effect: electron and nucleus orbit a common centre of mass that is not in the same place). The $\text{H}\alpha$ line at $656.28$ nm is thus doubled by a companion at $656.10$ nm — a gap of $0.18$ nm. Nobel Prize in Chemistry, 1934.

**In the Sun**, deuterium does not last: a deuteron once formed is consumed in $\approx 1$ second. It is a fleeting intermediate, never a reservoir. The Sun in fact burned its initial stock *before it was even born*: deuterium fusion starts at $10^6$ K, ten times lower than ordinary hydrogen fusion, so any contracting protostar triggers it very early. That is the definition of the planet/star boundary: below **13 Jupiter masses**, an object never reaches the million degrees; above it, it burns its deuterium and becomes a brown dwarf.

**Its cosmological value.** Deuterium is manufactured **nowhere** in the present universe — stars can only destroy it. All existing deuterium dates from the first three minutes. Its abundance depends very sensitively on the density of ordinary matter at the time of the Big Bang: it is the best "baryometer" available.

### The proton-proton chain

Branch pp-I, $86\%$ of the Sun's energy:

| Step | Reaction | Energy | Typical timescale |
|---|---|---|---|
| 1 | $p + p \rightarrow \text{D} + e^+ + \nu_e$ | $1.44$ MeV | $\approx 10^9$ years |
| 2 | $\text{D} + p \rightarrow {}^3\text{He} + \gamma$ | $5.49$ MeV | $\approx 1$ s |
| 3 | ${}^3\text{He} + {}^3\text{He} \rightarrow {}^4\text{He} + 2p$ | $12.86$ MeV | $\approx 400$ years |

Steps 1 and 2 happen **twice** to supply the two ${}^3\text{He}$ of step 3. Net balance:

$$4\,{}^1\text{H} \longrightarrow {}^4\text{He} + 2e^+ + 2\nu_e + 26.73\ \text{MeV}$$

The positrons annihilate instantly (their contribution is already counted). The neutrinos carry off $0.27$ MeV each on average and **leave the Sun in two seconds**; $\approx 26.2$ MeV remains, deposited as heat.

**Step 1 is the bottleneck**, and the spread of timescales is seventeen orders of magnitude. Two protons that meet cannot stay together since the diproton does not exist; at the precise moment of contact, one of the two must transmute:

$$p \longrightarrow n + e^+ + \nu_e$$

This is an inverse beta decay, governed by the **weak interaction**. It is so improbable that a given proton in the solar core waits a billion years on average, despite $10^{14}$ collisions per second. That bottleneck settles everything: the Sun's longevity, its low power per unit volume, and the impossibility of reproducing the pp chain in the laboratory — terrestrial reactors use D $+$ T, which does not go through the weak interaction.

**The other routes.** pp-II ($14\%$) goes via ${}^7\text{Be}$ and ${}^7\text{Li}$. pp-III ($0.02\%$) goes via ${}^8\text{B}$ and emits the **high-energy neutrinos** — the only ones the early experiments (Davis, Super-Kamiokande) could detect; it is this tiny branch that revealed the solar neutrino problem, and then neutrino oscillation. The **CNO cycle** ($\approx 1\%$ in the Sun) uses carbon as a catalyst: it captures four protons in turn and spits out a ${}^4\text{He}$ while regenerating itself, for an identical balance. It dominates above $\approx 1.3\,M_\odot$. Borexino detected its neutrinos in 2020.

### The mass defect: nothing is ejected

Nuclei do not separate, they **fuse** — and the product **stays put**. Helium has been accumulating in the core for $4.6$ Gyr, taking it from $71\%$ to $34\%$ hydrogen. No matter leaves the Sun through this mechanism.

The mass accounting:

- 4 hydrogen atoms: $4 \times 1.007825 = 4.031300$ u
- 1 helium-4 atom: $4.002602$ u
- Difference: $0.028698$ u, i.e. $\mathbf{0.7\%}$

The helium nucleus is **lighter** than the four protons it came from. That mass did not go anywhere as particles: it became energy, per $E = mc^2$.

> **Mass is not conserved in nuclear reactions — energy is.** A bound system weighs *less* than the sum of its constituents, because binding energy is negative and counts in the mass.

This holds even in chemistry: a hydrogen atom weighs slightly less than a separated proton plus electron, by $13.6\ \text{eV}/c^2$. In the nuclear case the effect is a million times larger and becomes measurable.

**Two quantities not to confuse:**

| Quantity | Value | Fraction of the Sun |
|---|---|---|
| Hydrogen **converted** into helium since the start | $8.9\times 10^{28}$ kg | $\approx 4.5\%$ |
| Mass actually **turned into energy** ($0.7\%$ of the above) | $6.2\times 10^{26}$ kg | $\approx 0.03\%$ |

The $4.5\%$ is mass *processed*, not lost: it is still there, as helium. What the Sun has actually lost is thirty times less.

**The current rate**, per second: $\approx 600$ million tonnes of hydrogen fused, $\approx 596$ million tonnes of helium produced, $\approx 4.3$ million tonnes vaporised into energy (exactly $L_\odot/c^2$). A curiosity: the Sun loses **more mass through radiation** ($4.3$ Mt/s) than through the solar wind ($1.5$ Mt/s). Light weighs.

### Gravity lights the star, fusion stops it

This is the most widespread confusion about stars: fusion **does not trigger** the heating, it interrupts it.

1. **A cold molecular cloud** ($10$–$20$ K), diffuse. Nothing happens in it.
2. **Gravitational collapse**, triggered by a perturbation — a supernova blast, the passage of a spiral arm.
3. **Gravity heats.** This is where the heat is born: potential energy released by infall becomes agitation, hence temperature. The virial theorem says half is radiated, half stays as heat.
4. **At $10^6$ K**, residual deuterium ignites briefly. The stock is soon exhausted.
5. **At $\approx 10^7$ K**, hydrogen fusion starts.
6. **Contraction stops.** The star settles into hydrostatic equilibrium and joins the main sequence.

> **Fusion does not light the star — it stops the star from collapsing further.**

Without it, the Sun would still shine by contraction alone (Kelvin-Helmholtz), but for $30$ million years instead of $10$ billion. Fusion is not the source of the heat, it is the source of the **duration**.

**The thermostat.** Once established, the equilibrium is self-regulating: if fusion runs away, the core heats up, **expands**, cools, and fusion slows. Negative feedback with a time constant of millions of years. That is why luminosity varies by only $0.1\%$ over an 11-year cycle, for a power of $3.83\times 10^{26}$ W.

**A surprise of scale.** Because of the weak-interaction bottleneck, the power released per unit volume in the Sun's core is only $\approx 280$ W/m³, i.e. $\approx 0.002$ W/kg. A human body produces $\approx 1.3$ W/kg, almost a thousand times more. **The Sun shines not because it is an intense reactor but because it is enormous** — and it is that sluggishness that gives it its endurance.

### The energy's journey, from core to Earth

**a) The core.** Gamma photons ($\approx 1$–$2$ MeV), positrons annihilated at once, kinetic energy thermalised by collisions. Neutrinos escape immediately.

**b) The random walk — and the degradation.** The gamma photon travels $\approx 1$ mm before being absorbed, then re-emitted in an arbitrary direction. It starts over billions of billions of times and takes $\approx 10^5$ years to drift to the surface. At each exchange it equilibrates with the **local** temperature, which falls as it rises: a single $1$ MeV gamma ends up converted into **$\approx 5\times 10^5$ visible photons** of $2$ eV. Energy is conserved, but fragmented and degraded. That is why the Sun shines yellow and not in gamma rays.

**c) The convective zone**, then **the photosphere**, where the Planckian continuum forms and escapes — see [[spectroscopy-and-black-body.en]] and [[structure-of-the-sun.en]].

**d) $8$ min $20$ s.** The contrast that sums everything up: the **neutrinos** passing through you right now were made **8 minutes** ago; the **photons** you see right now were made **$\approx 100\,000$ years** ago. They come from the same reaction.

**e) Arrival on Earth.** Radiation is the only possible mechanism: conduction and convection need matter, which the vacuum removes. The luminosity $L_\odot = 3.83\times 10^{26}$ W, diluted over a sphere of radius $1$ AU:

$$\frac{L_\odot}{4\pi d^2} = \frac{3.83\times 10^{26}}{4\pi (1.496\times 10^{11})^2} = 1\,361\ \text{W/m}^2$$

That is the solar constant. The Earth intercepts $\pi R_\oplus^2 = 1.27\times 10^{14}$ m² of it, i.e. $1.74\times 10^{17}$ W — about $10^4$ times humanity's energy consumption. Roughly $30\%$ is reflected (albedo), $70\%$ absorbed.

On absorption, the photon excites an electron or sets a molecule vibrating; the energy redistributes by collisions within picoseconds into disordered agitation. A point of vocabulary that matters: the photon does not carry heat, it carries **energy**, which becomes heat as it thermalises.

At equilibrium, absorbed $=$ emitted:

$$T = \left[\frac{S(1-A)}{4\sigma}\right]^{1/4} = 255\ \text{K} = -18\ ^\circ\text{C}$$

The missing $33\ ^\circ$C relative to the observed $+15\ ^\circ$C is the **greenhouse effect**: light arrives in the visible (peaking at $0.50\ \mu$m by Wien's law for $5\,772$ K) and crosses the atmosphere easily; the Earth radiates back in the far infrared (peaking at $\approx 11\ \mu$m for $255$ K), where $\text{CO}_2$, water vapour and methane absorb. It is this latitude-dependent differential heating that sets the atmosphere in motion — see [[general-atmospheric-circulation.en]].

The solar wind does bring matter, but its energy flux is $\approx 3\times 10^{-4}$ W/m², **ten million times less** than the radiation. Thermally negligible; its effect is magnetic.

### The rest of the stellar cycle

In $\approx 5$ Gyr, the core's hydrogen will be exhausted. The core, deprived of its pressure source, will contract and heat — gravity takes over again, exactly as at the beginning. Fusion will move to a shell around the core, the envelope will swell enormously (**red giant**, far enough to swallow Mercury and Venus). Then helium will ignite at $10^8$ K, abruptly (*helium flash*), producing carbon and oxygen. Finally the envelope will be blown off as a **planetary nebula**, leaving a degenerate carbon core the size of the Earth: a **white dwarf**, which will cool for trillions of years.

The Sun is too light to go further. No iron, no supernova.

### Where the hydrogen comes from

**$t < 10^{-6}$ s** — a quark-gluon plasma, too hot for protons and neutrons to exist.

**$t \approx 10^{-6}$ s, $T \approx 10^{13}$ K** — expansion cools the medium below the confinement threshold; quarks bind into triplets. **And a bare proton is already a hydrogen nucleus.** Hydrogen is the first chemical element, appearing before any star and any galaxy.

**$t \approx 1$ s** — neutrinos decouple, the interconversion stops, and the ratio freezes at $\approx 1$ neutron per $6$ protons (an asymmetry due to the neutron's slightly larger mass). Free neutrons then decay with $T_{1/2} = 10.2$ min; by the time of nucleosynthesis the ratio has fallen to $\approx 1{:}7$.

**$t \approx 3$ min, $T \approx 10^9$ K — the deuterium bottleneck breaks.** Building helium requires going through deuterium; but deuterium is so weakly bound ($2.22$ MeV) that as long as the universe is too hot, every deuteron formed is smashed by a photon. Nucleosynthesis is blocked for three minutes, not for lack of material but because the **intermediate does not survive**. Around $10^9$ K the photons are finally soft enough, the lock breaks, and everything follows within minutes.

**The result, frozen forever** (by mass):

| Species | Primordial abundance |
|---|---|
| ${}^1\text{H}$ | $\approx 75\%$ |
| ${}^4\text{He}$ | $\approx 25\%$ |
| D | $\approx 10^{-5}$ |
| ${}^3\text{He}$ | $\approx 10^{-5}$ |
| ${}^7\text{Li}$ | $\approx 10^{-9}$ |
| everything else | **zero** |

Synthesis stops dead for two reasons: there is **no stable nucleus of mass 5 or mass 8**, which breaks the assembly line; and density falls so fast with expansion that the reactions shut off. Carbon, oxygen, iron — none of it existed yet. It would take stars, and the triple collision of three helium nuclei, to bridge those gaps.

**$t \approx 380\,000$ years — recombination.** Until then the protons are bare, bathed in free electrons. At $\approx 3\,000$ K the electrons finally bind: the universe stops being a plasma and becomes **transparent**. The photons released at that instant are exactly those of the cosmic microwave background.

So, to be precise: **hydrogen nuclei are $13.8$ billion years old minus a microsecond; hydrogen atoms are $13.8$ billion years old minus $380\,000$ years.**

**How we can be sure.** Big Bang nucleosynthesis has only **one free parameter**, the density of ordinary matter, and with it predicts the abundances of D, ${}^3\text{He}$, ${}^4\text{He}$ and ${}^7\text{Li}$ — which span **nine orders of magnitude**. That same parameter is measured independently in the acoustic peaks of the cosmic microwave background (Planck), and the two values coincide: two unrelated pieces of physics, one nuclear at 3 minutes, the other acoustic at $380\,000$ years, point to the same number. Deuterium, which no star manufactures, provides the sharpest constraint when measured in pristine intergalactic clouds.

**One honest caveat**: the observed ${}^7\text{Li}$ is about three times lower than predicted. The "lithium problem" is unresolved.

**How that hydrogen ended up in the Sun.** It did almost nothing for thirteen billion years: the gas cooled, gathered in dark matter potential wells, formed galaxies and then molecular clouds. Generations of stars lit up and died in them, consuming only a small fraction but injecting the heavy elements they had forged at each death. $4.6$ Gyr ago, one of those clouds contained $1.3\%$ heavy elements — the accumulated contribution of every earlier star.

We even know a supernova probably triggered the collapse: some primitive meteorites contain the decay products of **aluminium-26** ($T_{1/2} = 717\,000$ years) and **iron-60**. Those nuclei do not survive more than a few million years — their presence proves they were injected just before or during the formation of the solar system.

## Examples & analogies

- **A water molecule joins two epochs.** The hydrogen formed in the universe's first microsecond and has never changed since. The oxygen it is bound to was made in the cores of massive stars that died a few billion years ago. Two atoms out of three in every water molecule in your body are $13.8$ billion years old.
- **The Sun is a bad reactor but a big one.** $0.002$ W/kg — a compost heap or a human body does better per kilogram. All its power comes from its volume, and all its longevity from its mediocrity per unit volume.
- **Kelvin against Darwin.** Kelvin, knowing only gravitational contraction, computed a $30$-million-year-old Sun and concluded that species evolution had not had time to happen. His physics was right, his inventory of energy sources incomplete.

## Open questions

- The **lithium-7 problem** (a factor 3 between prediction and observation) remains open.
- Stellar nucleosynthesis beyond helium (triple-alpha process, $\alpha$ capture, $s$ and $r$ processes) was not covered and would deserve its own sheet.
- Neutrino oscillations, only touched on via the solar neutrino problem.
- Controlled fusion on Earth (D $+$ T, tokamaks, inertial confinement) and why it cannot copy the pp chain.
- Detailed stellar evolution after the main sequence.

## Related notes

- [[structure-of-the-sun.en]]
- [[spectroscopy-and-black-body.en]]
- [[solar-wind.en]]
- [[general-atmospheric-circulation.en]]
