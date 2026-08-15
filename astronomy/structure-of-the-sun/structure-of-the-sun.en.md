---
title: "Structure and Composition of the Sun"
aliases: [Photosphere, Chromosphere, Solar corona, Coronal heating, Helioseismology, FIP effect]
tags: [astronomy, sun, stellar-physics, helioseismology]
created: 2026-08-11
source: conversation with Claude
lang: en
translations:
  - "[[structure-of-the-sun.fr]]"
related:
  - "[[spectroscopy-and-black-body.en]]"
  - "[[solar-eclipses.en]]"
  - "[[nuclear-fusion-and-origin-of-matter.en]]"
  - "[[solar-wind.en]]"
---

# Structure and Composition of the Sun

## TL;DR

> The Sun is a continuous gradient of gas, from a core at $15.7$ million K out to the interplanetary vacuum; its "surface" is not material but **optical** — the level where the gas stops being opaque. Its most stubborn oddity: the temperature **rises again** above the photosphere, up to $1$–$3$ million K in the corona, and nobody knows exactly why.

## Key concepts

- **Photosphere** — a surface of opacity ($\tau = 1$ at $500$ nm), not of matter; only $\approx 500$ km thick.
- **Tachocline** — a thin shear layer at $0.7\,R_\odot$ between rigid and differential rotation; the likely seat of the solar dynamo.
- **Transition region** — a few hundred km in which the temperature leaps from $20\,000$ K to $10^6$ K.
- **Coronal heating problem** — the corona is $\approx 300$ times hotter than the surface beneath it; the mechanism is unsettled.
- **Helioseismology** — reconstruction of the solar interior from its acoustic normal modes.
- **FIP effect** (*First Ionization Potential*) — the corona is enriched by a factor 3 to 4 in low-ionisation-potential elements.
- **Limb darkening** — observational proof that the "surface" is optical.

## Deep dive

### Overall composition

| | By mass | By atom count |
|---|---|---|
| Hydrogen | $73.8\%$ | $91.2\%$ |
| Helium | $24.9\%$ | $8.7\%$ |
| Everything else | $1.3\%$ | $0.1\%$ |

The "everything else": oxygen $0.77\%$, carbon $0.29\%$, iron $0.16\%$, then neon, nitrogen, silicon, magnesium, sulphur.

But this composition **is not uniform**, and the deviations are instructive (see "Two subtleties" below).

### The interior

**Core** — from $0$ to $0.25\,R_\odot$. $T = 15.7$ million K, density $150$ g/cm³ (thirteen times lead, yet it is a gas: the pressure, $250$ billion atmospheres, overrides everything). The seat of fusion. Its composition has **drifted**: $4.6$ billion years took it from $\approx 71\%$ to $\approx 34\%$ hydrogen, with $64\%$ helium. The Sun burns from the inside out.

**Radiative zone** — $0.25$ to $0.7\,R_\odot$. Energy travels as photons on a random walk, taking of order $10^5$ years to cross. $T$ falls from $15$ to $2$ million K. This medium rotates **as a block**, like a solid.

**Tachocline** — a very thin shear layer at $0.7\,R_\odot$, where the rigid rotation below meets the differential rotation above. This is probably where the **solar dynamo** is born, hence the 11-year cycle, hence the sunspots. Discovered by helioseismology in the 1990s.

**Convective zone** — $0.7\,R_\odot$ up to the surface. The gas becomes cool enough for atoms to partially recombine, opacity explodes (mainly through the $\text{H}^-$ ion), radiative transport chokes and the medium starts to boil. We see its top: the **granulation**, cells of $\approx 1\,000$ km living $\approx 10$ minutes, and supergranulation at $\approx 30\,000$ km.

### The atmosphere

**Photosphere** — a film $\approx 500$ km thick ($0.07\%$ of the radius). Density $\approx 10^{-7}$ g/cm³, a thousand times less than air. $T$ from $6\,400$ K at the base to $4\,400$ K at the top, for an effective temperature of $5\,772$ K. Granules, sunspots ($3\,800$ K, held back by their own magnetic field), faculae. It is the photosphere that produces the Fraunhofer absorption lines.

**Temperature minimum** — around $500$ km altitude, $\approx 4\,100$ K. Cool enough for **molecules** to survive: CO, and even traces of water. The coldest point of the Sun.

**Chromosphere** — $\approx 2\,000$ km, density $\approx 10^{-12}$ g/cm³. The temperature **rises again**, from $4\,100$ to $\approx 20\,000$ K. Highly structured, bristling with **spicules** (jets of $10\,000$ km rising at $20$ km/s, hundreds of thousands at any moment). Red in $\text{H}\alpha$, hence its name.

**Transition region** — a few hundred km, sometimes less than $100$. $T$ leaps from $20\,000$ K to $10^6$ K. An extremely ragged, fluctuating surface, not a smooth shell. It radiates in the extreme ultraviolet (C IV, O VI, Si IV) — invisible from the ground, which is why it was only discovered with rockets and satellites.

**Corona** — $1$ to $3$ million K, up to $20$ MK in active regions. Density at its base $\approx 10^8$ particles/cm³, i.e. $10^{-15}$ g/cm³: a better vacuum than we can make in the laboratory. Its geometry is **entirely dictated by the magnetic field**: closed loops, helmet streamers, polar plumes, and **coronal holes** where the field lines are open.

Beyond that, the corona never stops — see [[solar-wind.en]].

### The "surface" of a ball of gas

There is no material surface at all. What we call the surface is an **optical surface**: the level where the optical depth reaches $\tau = 1$ at $500$ nm. Below it, a photon has less than a $1/e$ chance of escaping; above it, it streams away freely.

**Why it is so sharp.** Photospheric opacity is dominated by $\text{H}^-$, whose abundance depends on the number of free electrons, which in turn depends very steeply on temperature through the Saha equation. The transition from opaque to transparent happens over $\approx 100$ km, on a radius of $696\,000$ km — a ratio of $1.4\times 10^{-4}$.

Scaled down: **if the Sun were a one-metre balloon, the photosphere would be $0.15$ mm thick.** A skin. Hence the razor-sharp edge in photographs, even though it is gas.

**The observational proof** is **limb darkening**. At the centre of the disc, the line of sight plunges vertically and reaches down into hot layers; at the limb it looks obliquely, and $\tau = 1$ is reached higher up, hence in cooler gas, hence less bright. A solid surface would produce nothing of the kind — and the precise profile of that darkening lets you reconstruct the temperature gradient.

### The coronal heating problem

**How it was established.** It is not one measurement but a convergence over several decades, and the result was fiercely resisted.

*The "coronium" puzzle (1869–1941).* A green line at $530.3$ nm matched nothing known; a new element was postulated. The puzzle lasted seventy years, until Grotrian (1939) and Edlén (1941) identified Fe X ($637.4$ nm) and Fe XIV ($530.3$ nm). These are **forbidden transitions**, whose probability is so low that they only appear in a gas tenuous enough for an excited ion to go seconds without a collision. Stripping 13 electrons off iron requires, via the Saha equation, $1$ to $3$ million kelvin.

*Line widths.* Thermal agitation broadens lines by the Doppler effect, $v = \sqrt{2kT/m}$. For iron at $2$ MK this gives $\approx 24$ km/s, exactly what is measured; at $6\,000$ K the lines would be twenty times narrower.

*The absence of lines in the coronal continuum.* The K corona is photospheric light scattered by free electrons: it ought to carry the Fraunhofer lines, and it has none. Electrons, being very light, have $\sqrt{2kT/m_e} \approx 7\,800$ km/s at $2$ MK, i.e. a Doppler shift of $2.6\%$ — $13$ nm at $500$ nm, more than enough to smear out lines a few tenths of a nm wide. **The erasure of the lines *is* the thermometer.**

*The scale height.* An atmosphere in hydrostatic equilibrium decays over $H = \dfrac{kT}{\mu m_H g}$. With $g_\odot = 274$ m/s²: at $6\,000$ K, $H \approx 300$ km — a film; at $2$ MK, $H \approx 10^5$ km. The mere **geometric extent** of the corona, visible to the naked eye during an eclipse, already demands the million degrees.

*Radio astronomy (1946).* Pawsey, Martyn and others measured a brightness temperature of $\approx 10^6$ K at metre wavelengths, which can only emerge from the corona. A **direct** measurement, with no spectroscopic assumption — it is what won the sceptics over.

*X-rays (from 1949).* Burnight and then Friedman, using V-2 rockets, detected solar X-ray emission, impossible at $6\,000$ K. Skylab imaged it in 1973 and revealed the coronal holes.

**This is not a violation of thermodynamics.** The corona is not heated by conduction from the photosphere. Energy arrives there in **non-thermal** form — magnetohydrodynamic waves and field reconnection — which crosses the cool layers without dissipating in them, then degrades into heat up there. The usual analogy is the fluorescent tube, whose plasma reaches $10\,000$ K behind a lukewarm glass wall: it is the electric current, not conduction, that carries the energy.

**The mechanism remains open.** Two families: **nanoflares** (Parker 1988 — billions of micro-reconnections) and **Alfvén wave dissipation**. The answer is probably a region-dependent mix. It is the central goal of Parker Solar Probe and Solar Orbiter.

### Helioseismology

**The source.** The convective zone boils permanently; plasma plunging down the intergranular lanes, sometimes supersonically, acts like so many acoustic hammer blows. The excitation is **stochastic and broadband**: the Sun rumbles continuously, like a waterfall.

**The cavity.** A sound wave heading down meets an ever hotter medium, where the sound speed ($\propto \sqrt{T}$) increases: it is progressively **refracted** and eventually turns back up — the same principle as a mirage. On reaching the photosphere, the density drop reflects it back inwards through an acoustic impedance break. Trapped between two mirrors, only the frequencies forming standing waves survive: the **normal modes**.

They cluster around a $5$-minute period ($3$ mHz). Discovered by Leighton, Noyes and Simon in 1962 — who thought them local — they were identified as **global** oscillations by Ulrich (1970) and Leibacher & Stein (1971), then confirmed by Deubner in 1975.

**The inversion.** Each mode reaches a different depth depending on its horizontal wavelength, and reports the sound speed averaged along *its* path. With $\approx 10^7$ modes detected, you reconstruct $c(r)$, then temperature, density, mean molecular weight and **rotation** layer by layer. That is how the tachocline was discovered.

The amplitudes are staggeringly small: an individual mode displaces the surface at $\approx 10$ cm/s. They are separated by Fourier analysis of time series several years long (GONG on the ground, SOHO/MDI then SDO/HMI in space), measuring the Doppler shift of a line formed at a known altitude.

**One frustration.** All of this concerns the **$p$ modes** (pressure, acoustic). There should also be **$g$ modes** (gravity, with buoyancy as the restoring force), far more sensitive to the nuclear core — but they are evanescent in the convective zone, which almost entirely smothers them. No detection has been confirmed. It is one of the great unfinished goals of solar physics.

### Two subtleties of composition

**Gravitational settling.** Over $4.6$ Gyr, helium and heavy elements have slowly diffused inwards. The photosphere is depleted by about $10\%$ relative to the initial composition. Helioseismology confirms this effect precisely.

**The FIP effect.** The corona does not have the composition of the photosphere. Elements whose first ionisation potential is below $\approx 10$ eV (iron, magnesium, silicon, calcium) are enriched there by a factor 3 to 4, while high-FIP elements (oxygen, neon, helium) are not. The sorting happens in the chromosphere, where low-FIP elements are already ionised and therefore sensitive to magnetic forces, unlike neutrals. It is a precious tracer: the composition of the solar wind tells you which coronal region it came from.

### How we know the Sun is made of hydrogen and helium

Six chains of reasoning **with no shared assumption**, six times the same answer:

1. **The spectroscopic reversal (1925).** Cecilia Payne applied the Saha equation and showed that a line's intensity measures *abundance $\times$ excitation factor*. Once corrected, hydrogen comes out $\approx 10^6$ times more abundant than the metals. Russell reached this independently in 1928–1929, as did Unsöld in 1928 from line profiles.
2. **The energy constraint.** Radiating $3.83\times 10^{26}$ W for $4.6$ Gyr requires $5.6\times 10^{43}$ J. Chemical burning would exhaust the Sun in a few thousand years; Kelvin-Helmholtz contraction lasts $30$ million years (this was Kelvin's argument against Darwin — and Kelvin was the one who was wrong). Only hydrogen fusion works, and it **requires** a massively hydrogen-rich Sun.
3. **Helioseismology.** The sound speed depends on mean molecular weight, hence on the helium fraction. The inversion gives $Y = 0.248 \pm 0.003$ in the convective envelope, independently of any spectroscopy. That is all the more valuable because helium is **spectrally invisible** in the photosphere: its abundance *could not* be measured by the method that discovered it. We had to listen to the Sun ringing.
4. **Direct sampling.** The Genesis mission (2001–2004) exposed collectors to the solar wind at L1 and brought them back; laboratory analysis, atom by atom, isotope by isotope. After FIP correction, it agrees with spectroscopy.
5. **Neutrinos.** Borexino directly measured the pp neutrino flux in 2014, consistent with the rate predicted for hydrogen fusion. Proof that the assumed reaction is happening, at the assumed rate, *now* — neutrinos leave the core in 2 seconds.
6. **External consistency.** Primordial nucleosynthesis predicts $75\%$ H / $25\%$ He by mass, measured independently in very metal-poor systems. And for non-volatile heavy elements, photospheric abundances coincide with those of CI chondrites measured by mass spectrometry.

**One unresolved anomaly**: the downward revision of carbon and oxygen abundances (Asplund et al. 2009), based on better 3D atmosphere models, **broke** the previously excellent agreement between solar models and helioseismology. The discrepancy persists.

## Examples & analogies

- **The cloud.** Diffuse vapour, with no wall or membrane — and yet, seen from a plane, a perfectly defined "top" you feel you could walk on. For exactly the same reason as the photosphere: that top is the level where the cloud becomes opaque. Same physics, same illusion of solidity.
- **The bell.** Helioseismology determines the Sun's interior from its harmonics, the way you would deduce a bell's structure from its overtones. A delicious difference from terrestrial seismology: on Earth you wait for an earthquake to probe the interior; on the Sun the noise source **never** stops — it probes itself continuously.
- **The fluorescent tube**: plasma at $10\,000$ K behind lukewarm glass. Heat gets there not by conduction but through the electric current. Same logic for the corona, with magnetism in place of the current.

## Open questions

- **Coronal heating** — nanoflares or Alfvén waves? Unsettled.
- **The $g$ modes** have never been convincingly detected; they would open a direct window on the core.
- **The post-Asplund abundance problem** remains open.
- The **solar dynamo** and the 11-year cycle (sunspots, Spörer's law, butterfly diagram, magnetic reversal) would deserve their own sheet.
- The Sun's post-main-sequence evolution is only sketched in [[nuclear-fusion-and-origin-of-matter.en]].

## Related notes

- [[spectroscopy-and-black-body.en]]
- [[solar-eclipses.en]]
- [[nuclear-fusion-and-origin-of-matter.en]]
- [[solar-wind.en]]
