---
title: "Spectroscopy and Black-Body Radiation: Reading Starlight"
aliases: [Fraunhofer lines, Kirchhoff's laws, Saha equation, Black body, Telluric lines]
tags: [astronomy, physics, spectroscopy, black-body, saha]
created: 2026-08-11
source: conversation with Claude
lang: en
translations:
  - "[[spectroscopy-and-black-body.fr]]"
related:
  - "[[solar-eclipses.en]]"
  - "[[structure-of-the-sun.en]]"
  - "[[nuclear-fusion-and-origin-of-matter.en]]"
---

# Spectroscopy and Black-Body Radiation: Reading Starlight

## TL;DR

> A dense hot gas emits a continuum that depends only on its temperature (a black body); cooler layers above carve dark lines into it at the wavelengths specific to each element. All of astronomical spectroscopy rests on one idea: **each wavelength shows you the stellar atmosphere at a different altitude**, and altitude sets temperature, hence brightness.

## Key concepts

- **Black body** — not a substance but a *state*: matter dense enough to thermalise radiation completely, whose spectrum then depends only on $T$.
- **Kirchhoff's laws** — dense and hot $\rightarrow$ continuum; rarefied gas against a dark background $\rightarrow$ emission lines; a continuum seen through a cooler gas $\rightarrow$ absorption lines.
- **Kirchhoff's law of thermal radiation** — at every wavelength, emissivity $=$ absorptivity: a good absorber is a good emitter.
- **Optical depth $\tau$** — a measure of cumulative opacity; $\tau = 1$ defines the visible "surface" at a given wavelength.
- **The altitude effect** — a line is dark not because light is missing, but because it shows you a higher, cooler layer.
- **Saha equation** — gives the ionisation fraction of a gas at equilibrium, as a function of $T$ **and** electron density.
- **Telluric lines** — absorption lines produced by the Earth's atmosphere, to be distinguished from stellar ones.
- **Flash spectrum** — the absorption $\rightarrow$ emission reversal observed at the contacts of a total eclipse.

## Deep dive

### Why lines exist

In an isolated atom, the electron occupies only **discrete levels** — the rungs of a ladder, not a ramp. Going from $E_1$ to $E_2$ requires exactly $E_2 - E_1$. Since a photon's energy is $E = h\nu = hc/\lambda$, an atom can only interact with precise wavelengths, set by the spacing of its rungs, itself set by its electronic structure. Hence the fingerprint unique to each element.

Kirchhoff and Bunsen established this in 1859–1860 by showing that the Sun's two $D$ lines fall exactly where sodium heated in the laboratory emits its own.

### Why a black body has no lines

In **dense** matter, atoms are packed together, undergo constant collisions and perturb one another. Instead of sharp rungs, the levels broaden until they merge into a continuum. On top of that come free electrons, which, being bound to nothing, can absorb or emit any energy at all.

Three mechanisms actually produce this continuum, and none involves discrete levels:

- **free–free (bremsstrahlung)** — a free electron deflected by an ion is accelerated and radiates; since it can lose any fraction of its kinetic energy, it emits at any wavelength;
- **free–bound (recombination)** — an ion captures a free electron; the photon carries away the binding energy (fixed) *plus* the electron's initial kinetic energy (arbitrary);
- **the $\text{H}^-$ ion** — dominant in the Sun. A neutral hydrogen atom can hold a second electron, very weakly ($0.75$ eV). Any photon of higher energy — i.e. $\lambda < 1.65\ \mu$m, which covers the whole visible range — can tear it off, and the reverse process emits continuum. It is $\text{H}^-$ that makes the Sun's surface opaque, and therefore produces the light we see. Understood only in 1939 (Wildt).

The exactly Planckian shape comes from **churning**: in an optically thick medium a photon is absorbed and re-emitted billions of times before escaping, and at each exchange it forgets its origin and conforms to local conditions. The radiation field loses all memory of composition and depends only on $T$.

Two practical consequences:

$$\lambda_{\max} = \frac{2898\ \mu\text{m}\cdot\text{K}}{T} \qquad\text{(Wien)} \qquad\qquad F = \sigma T^4 \qquad\text{(Stefan–Boltzmann)}$$

The word "black" comes from Kirchhoff's law of thermal radiation: at every wavelength, emissivity $=$ absorptivity. The perfect thermal radiator is therefore the perfect absorber.

### Absorption: the altitude effect

The immediate mechanism is **directional scattering**. A photon at $589.0$ nm heads for Earth; a sodium atom absorbs it, then re-emits an identical photon in a random direction over $4\pi$ steradians. That photon is lost *to us*, not to the universe. Nothing is destroyed.

But the real accounting lies elsewhere, and this reformulation unlocks everything:

> **A dark line is not missing light — it is light from a cooler layer.**

At $500$ nm (outside the line), the gas above the photosphere is transparent: the line of sight plunges deep and stops at $6\,400$ K. At $589.0$ nm (the sodium line), that same gas is extremely opaque: the line of sight stops much higher, at $4\,400$ K. Since brightness goes as $T^4$, the ratio is $(6400/4400)^4 \approx 4.5$.

That is why the bottom of a Fraunhofer line is **never black**: a residue always remains, from the high layer shining at its own temperature.

The "missing" energy is not lost: it is scattered elsewhere, or converted by collisions and re-emitted at other wavelengths. The massive flux blocking by metal lines in the blue even heats the underlying layers (**backwarming**) and comes back out shifted towards the red. Total flux is rigorously conserved.

### The instrument chain

```
slit         → cuts out a thin line (defines the object)
collimator   → makes the rays parallel
grating      → sends each λ in a different direction
camera lens  → converts direction → position on the detector
```

**The slit does not disperse** and, ideally, does not diffract: its role is geometric. A spectrograph forms an image of the source once per wavelength, shifting each image sideways. If the source is broad, the shifted images overlap and average into mush — that is the smooth rainbow of a hand-held prism. If the source is a thin line, each wavelength gives a sharp line. **A spectral line is literally an image of the slit**; its shape comes from the slit, not from atomic physics. The exact analogy is the pinhole camera, in one dimension.

The slit does diffract anyway, and that is a defect: too narrow, it spreads the beam and degrades resolution while losing light. There is an optimal width, typically a few tens of $\mu$m.

**The grating disperses.** Its grooves, spaced by $d$ (typically 600 to 1200 lines/mm, so $d$ between $1.7$ and $0.8\ \mu$m — comparable to $\lambda$, and that is why it works), each send light in every direction. Waves from neighbouring grooves interfere constructively only if their path difference is a whole number of wavelengths:

$$d\sin\theta = m\lambda$$

Each colour therefore emerges at its own angle; everywhere else the waves cancel. The camera lens then converts angle into position: **on the detector, the horizontal coordinate is a wavelength**.

The resolving power is $R = \lambda/\Delta\lambda = mN$ ($m$ the order, $N$ the number of illuminated grooves). Order $m=0$ does not disperse; high orders disperse more but overlap. **Échelle** spectrographs deliberately work at $m \approx 50$–$100$ and add a cross-disperser to stack the orders: that is the architecture of $R \approx 10^5$ instruments, the ones that detect exoplanets by radial velocity.

Because the Sun is very bright, one can afford $R \approx 10^6$ without running out of photons — a luxury unavailable on distant stars.

### The Saha equation

It gives the fraction of ionised atoms at thermal equilibrium:

$$\frac{n_{i+1}\,n_e}{n_i} = 2\,\frac{g_{i+1}}{g_i}\left(\frac{2\pi m_e k T}{h^2}\right)^{3/2} e^{-\chi_i / kT}$$

Two terms compete:

- **$e^{-\chi/kT}$** — the Boltzmann factor: the probability that a collision supplies the ionisation energy. It grows abruptly with $T$.
- **$T^{3/2}/n_e$** — the **entropy** term: a free electron has a gigantic state space compared with a bound one. Hence the counter-intuitive result: **ionisation is favoured by low density**, not only by heat.

One number makes this tangible. In the photosphere, $kT \approx 0.5$ eV whereas ionising hydrogen costs $\chi = 13.6$ eV:

$$e^{-13.6/0.5} = e^{-27.2} \approx 1.5\times 10^{-12}$$

Yet the measured value is $\approx 10^{-4}$. The eight-order-of-magnitude gap comes **entirely from the entropy term**: with $n_e \approx 10^{19}$ m⁻³, the prefactor is $\approx 10^{8}$, and $10^{8}\times 1.5\times 10^{-12} \approx 1.6\times 10^{-4}$. It is also why a stellar atmosphere, being very rarefied, is far more ionised than a laboratory gas at the same temperature — and why the corona reaches such extravagant ionisation states.

**Origins.** Meghnad Saha, in Calcutta in 1920, made a connection nobody had made: ionisation is a **chemical reaction**, $A \rightleftharpoons A^+ + e^-$. Once written that way, the law of mass action and van't Hoff's isochore apply. He combined three bodies of work that were not talking to each other — chemical thermodynamics (Nernst, van't Hoff), statistical mechanics to count the states of the freed electron, and the brand-new Bohr model which finally supplied the ionisation potentials. Fowler and Milne refined it in 1923–1924 by proposing to use a line's **intensity maximum** rather than its appearance or disappearance, a far more objective criterion.

**What it unlocked.** The Harvard sequence O B A F G K M, established empirically, became a sequence of **temperature** rather than composition. The Balmer lines peak in A stars ($\approx 9\,500$ K): any cooler and too few atoms have climbed to the $n=2$ level the series starts from; any hotter and hydrogen is ionised and has no electron left. Hydrogen is nevertheless the most abundant element everywhere.

It also served as a thermometer — that is how Edlén established the corona's million degrees — and its formal equivalent describes carrier concentration in a semiconductor, the entropic reasoning being identical.

**Its limit.** It assumes **local thermodynamic equilibrium**: everything governed by collisions, at a single temperature. Valid in the photosphere and inside stars, it breaks down in the chromosphere and corona, where density is so low that the radiation field — arriving from elsewhere, with its own "temperature" — drives ionisation more than collisions do. There you must solve the statistical equilibrium level by level ("non-LTE").

### Line strength $\neq$ abundance

This is the central trap of spectral analysis. Four factors act on top of composition:

- **temperature**, which decides which levels are populated. Helium is the extreme case: its first excited level sits at $19.8$ eV, the highest in the periodic table. At $5\,800$ K there are essentially no excited helium atoms — helium is **almost invisible** in the photospheric spectrum even though it makes up $25\%$ of the Sun's mass;
- **ionisation state**: Fe I, Fe II, … Fe XIV are *different* atomic systems, with different lines;
- **density**, which broadens lines by collisions — that is how a giant is told apart from a dwarf;
- **velocity** (Doppler) and the **magnetic field** (Zeeman, which splits lines in proportion to $B$ — that is how Hale discovered in 1908 that sunspots carry $\approx 3\,000$ gauss).

The historical counter-example is decisive. Around 1920, stars were believed to be composed like the Earth, because iron and calcium dominate the solar spectrum visually. **Cecilia Payne**, in 1925, applied Saha and showed that a line's strength measures the product *abundance $\times$ excitation factor*. Once corrected, the verdict lands: hydrogen is $\approx 10^6$ times more abundant than the metals by atom count. The result was so contrary to consensus that she was made to add that it was "almost certainly not real". It was right.

### Telling solar lines from terrestrial ones

Lines produced by our own atmosphere really do exist: the **telluric lines**. Five independent methods separate them unambiguously:

1. **Air mass.** Near the horizon, light crosses up to 40 times more atmosphere: telluric lines deepen markedly, solar lines do not budge. A historical test, carried out by Janssen in the 1860s — he coined the word "telluric", and climbed to the summit of Mont Blanc to minimise water vapour.
2. **Doppler shift.** The Earth orbits at $30$ km/s: over six months every solar line moves by $\pm 0.5$ Å while the telluric lines, produced by gas at rest relative to the spectrograph, stay rigorously fixed. An elegant variant: the Sun's eastern limb approaches at $\approx 2$ km/s and the western limb recedes, so moving the slit from one limb to the other makes solar lines swing while telluric lines do not flinch.
3. **The identity of the species.** Telluric lines come from **molecules** — $\text{O}_2$, $\text{H}_2\text{O}$, $\text{CO}_2$, $\text{O}_3$ — which would be dissociated instantly at $5\,800$ K. Their signature is recognisable: they are **bands** (clusters of hundreds of rotation-vibration lines), not isolated lines.
4. **Comparing objects.** Two stars observed the same night in the same direction show identical telluric lines and different stellar ones.
5. **Space.** Above the atmosphere they simply disappear.

A delicious historical detail: when Fraunhofer labelled his lines A to K in 1814, his first two — band **A** at $759$ nm and band **B** at $686$ nm — are oxygen from our own atmosphere. The founding catalogue of astrophysics opens with two terrestrial lines.

### The flash spectrum: the demonstration

Before 2nd contact of a total eclipse, the line of sight crosses the chromosphere and reaches the photosphere: **a bright continuum striped with dark lines**. Afterwards, with the Moon covering the photosphere, the chromosphere stands out against a dark background: **a dark background striped with bright lines**, at exactly the same wavelengths.

> **Nothing changed in the gas. Only the background changed.**

The atoms were already emitting towards the observer before the eclipse; it is simply that their own emission was weaker than the continuum they were blocking at the same place. The balance was negative. Remove the continuum and only their emission is left. The "reversal" is a subtraction, not a change of physics — it is Kirchhoff's law made visible live.

Kirchhoff had **postulated** in 1859 that the Fraunhofer lines arise in a relatively cool layer above the source of the continuum. Young **saw** it in 1870. All of stellar astrophysics rests on that validation.

An observational bonus: the reversal is not simultaneous for all lines. Those formed very high (hydrogen, helium, the H and K lines of ionised calcium) flash first and last longest. Filming the flash spectrum at high cadence lets you **leaf through the solar atmosphere by altitude** — one of the few methods that directly yields the vertical stratification of the chromosphere.

## Examples & analogies

- **Fog.** A scorching ground topped by a cooler layer of fog: you do not see the ground, you see the top of the fog, which shines less. Solar fog is simply **wavelength-selective** — opaque in the lines, transparent elsewhere. Hence the altitude effect.
- **The blacksmith's furnace.** A black body is not invisible: cold, it looks perfectly black (it reflects nothing); hot, it glows. But at **thermal equilibrium** with its surroundings, emission and absorption cancel exactly at every wavelength and it becomes rigorously indistinguishable from the background. Potters live this daily: in a uniform-temperature kiln, relief and outlines dissolve into a homogeneous orange soup; you have to take the piece out to see it. What makes an object visible is always a **contrast**.
- **White-hot metal** is a thermometer: $850$ K dull red, $1\,300$ K cherry red, $1\,800$ K orange-yellow, $2\,800$ K white. Even "white hot", the peak stays in the infrared — what we see is only the tail of the curve, flat enough over $0.4$–$0.7\ \mu$m for the eye to integrate it as white. (A real metal is only a "grey body": emissivity $< 1$ and $\lambda$-dependent, hence the need to calibrate pyrometers.)

## Open questions

- Non-LTE radiative transfer (chromosphere, corona) was barely touched on.
- Detailed line profiles — Doppler and collisional broadening, Lorentz wings, curve of growth — would deserve a sheet.
- The revised solar abundance problem (Asplund 2009), which broke the agreement between models and helioseismology, remains unresolved.
- The Zeeman effect and solar magnetometry (spectropolarimetry) were not developed.

## Related notes

- [[solar-eclipses.en]]
- [[structure-of-the-sun.en]]
- [[nuclear-fusion-and-origin-of-matter.en]]
