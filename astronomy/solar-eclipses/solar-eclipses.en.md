---
title: "Solar Eclipses: Geometry and the Science of Totality"
aliases: [Total eclipse, Annular eclipse, Solar corona, Flash spectrum, Baily's beads]
tags: [astronomy, eclipse, sun, moon, general-relativity]
created: 2026-08-11
source: conversation with Claude
lang: en
translations:
  - "[[solar-eclipses.fr]]"
related:
  - "[[spectroscopy-and-black-body.en]]"
  - "[[structure-of-the-sun.en]]"
  - "[[solar-wind.en]]"
---

# Solar Eclipses: Geometry and the Science of Totality

## TL;DR

> Because the lunar orbit is elliptical, the Moon's disc is sometimes larger and sometimes smaller than the Sun's: hence total, annular or hybrid eclipses. Only *strict* totality reveals the chromosphere and the corona — and it was by observing them that helium was discovered, the corona's million degrees measured, and general relativity confirmed.

## Key concepts

- **Eclipse magnitude** — ratio of the apparent Moon/Sun diameters; $>1$ gives a total eclipse, $<1$ an annular one.
- **Hybrid eclipse** — total mid-track, annular at the ends, because of observer parallax.
- **Baily's beads** — the last rays filtering between the mountains of the lunar limb, just before and after totality.
- **Flash spectrum** — the chromospheric spectrum flipping from absorption to emission at 2nd and 3rd contact.
- **K / F / E corona** — scattering by free electrons / by dust / intrinsic emission lines.
- **PPN coefficient $\gamma$** — measures spatial curvature per unit mass; $\gamma = 1$ in general relativity, $\gamma = 0$ in Newtonian gravity.

## Deep dive

### The Earth–Moon distance is not constant

The lunar orbit is an ellipse of eccentricity $e \approx 0.055$:

| | Distance |
|---|---|
| Perigee | $\approx 356\,500$ km |
| Mean | $384\,400$ km |
| Apogee | $\approx 406\,700$ km |

That is a $\approx 50\,000$ km spread — four times the Earth's diameter. The ellipse itself is not fixed: solar perturbations (evection, variation) make the perigee distance oscillate between $356\,400$ and $370\,400$ km.

Two effects add to this. **Parallax**: an observer with the Moon at the zenith is one Earth radius closer to it ($6\,371$ km, i.e. $1.7\%$ of the distance) than one who sees it on the horizon. And **secular drift**: the Moon recedes by $3.8$ cm/year, measured by laser ranging off the Apollo retroreflectors.

### Three types of eclipse, and why

The Moon's apparent diameter varies from $29.4'$ to $33.5'$ ($\pm 6\%$). The Sun's varies far less, from $31.5'$ (aphelion, early July) to $32.5'$ (perihelion, early January), i.e. $\pm 1.7\%$ — the Earth's orbit being much more circular ($e \approx 0.017$).

The ratio of the two therefore sweeps roughly $0.90$ to $1.06$:

- **Total** ($>1$) — theoretical maximum duration $7$ min $32$ s; in practice 2 to 3 min. Path of totality up to $\approx 270$ km wide.
- **Annular** ($<1$) — a ring of photosphere survives, for up to $12$ min $30$ s.
- **Hybrid** ($\approx 1$) — the observer at the centre of the path, one Earth radius closer to the Moon, sees a total eclipse where the ends of the track see an annular one.

Annular eclipses are slightly more frequent ($\approx 33\%$ of solar eclipses) than total ones ($\approx 27\%$): the Moon's shadow cone is on average a little too short to reach the ground.

This coincidence is temporary. In $\approx 600$ million years the Moon will have receded far enough that no total eclipse is possible any more. With the Sun 400 times bigger and 400 times further away, we simply happen to live in the window where the two discs match.

### The contrast threshold: why strict totality is required

The corona has a surface brightness of about $10^{-6}$ that of the photosphere — comparable to the full Moon. If $1\%$ of the disc stays uncovered, it still sends $\approx 10^4$ times more light than the entire corona.

Consequence: an annular eclipse, or even a $99.9\%$ total one, teaches you **nothing** about the corona. The contrast flips within seconds at 2nd contact.

### Chromosphere visibility: a time window, not a distance

The chromosphere is $\approx 2\,000$ km thick on a solar radius of $696\,000$ km, i.e. $0.29\%$ — in angle, a band of only $\approx 2.8''$, whereas the Moon's excess radius can reach $60''$. During full totality it is therefore buried far below the lunar edge.

But it necessarily reappears at the contacts: since the chromosphere sits just above the photosphere, the instant the last scrap of photosphere disappears is also the instant the chromosphere is still exposed. The lunar limb advances at roughly $0.5''$ per second relative to the solar limb; it therefore takes a few seconds to sweep the $2.8''$. Hence a visibility of **1 to 3 seconds** at 2nd contact, then again at 3rd contact on the opposite limb.

What distance changes is comfort, not possibility. At the **edge of the path of totality**, the Moon is off-centre and one limb stays grazed: the chromosphere can remain visible there for almost the whole of totality. Prominences ($50\,000$–$100\,000$ km, i.e. $\approx 70''$) stick out permanently in any case.

Consistency check: the lunar *diameter* excess is at most $\approx 120''$, which at $0.5''$/s gives $\approx 4$ min — and the Earth's rotation, which carries the observer in the same direction as the shadow, stretches this to the theoretical $7$ min $32$ s. The same number governs both the duration of totality and the chromospheric window.

### What you see during totality

**Baily's beads and the diamond ring.** Timed precisely and cross-referenced with lunar topography (known to the metre since LRO), they are used to measure the **diameter of the Sun** and to look for variations with the cycle.

**Chromosphere.** A pink-red rim visible for a second or two, coloured by $\text{H}\alpha$ at $656.3$ nm.

**Prominences.** Loops of plasma anchored in the magnetic field.

**Corona.** Pearly, its structure *draws the magnetic field*: at solar minimum, two large equatorial helmet streamers and neatly combed polar plumes; at maximum, a squat corona radiating in every direction. Three components superimpose — **K** (scattering by free electrons, a line-free continuum), **F** (scattering by interplanetary dust, which preserves the Fraunhofer lines) and **E** (intrinsic emission lines).

### What eclipses have taught us

**Helium (1868).** During the eclipse of 18 August over India, Janssen and then Lockyer spotted a yellow line at $587.6$ nm (the $D_3$ line) matching no known element. It is the only chemical element discovered somewhere other than Earth before being found here — it would not be isolated in the laboratory until 1895.

An eclipse was needed because $D_3$ does **not** exist in the photospheric spectrum: it is a transition between two already excited levels, $\approx 20$ eV above the ground state, whereas $kT \approx 0.5$ eV in the photosphere. Helium, $25\%$ of the Sun's mass, is spectrally invisible there.

**The temperature of the corona.** The green line at $530.3$ nm, seen in 1869, was attributed to a hypothetical "coronium". The puzzle lasted seventy years, until Grotrian (1939, red line at $637.4$ nm $=$ Fe X) and Edlén (1941, green line $=$ Fe XIV, thirteen-times-ionised iron). These are **forbidden transitions**, possible only in an extremely tenuous gas. Ionising iron that far requires $1$ to $3$ million kelvin.

**General relativity (1919).** See below.

**Vulcan.** Eclipses were also used to hunt for the intra-Mercurial planet supposed to explain the advance of Mercury's perihelion. A null result — relativity settled the question in the end.

### The deflection of light: the calculation

**Newtonian prediction (Soldner, 1801).** Treating the photon as a corpuscle of speed $c$ on a hyperbola:

$$\alpha = \frac{2GM}{c^2 b}$$

where $b$ is the impact parameter. For a ray grazing the solar limb ($GM = 1.327 \times 10^{20}$ m³/s², $b = R_\odot = 6.957 \times 10^8$ m):

$$\alpha = 4.24 \times 10^{-6}\ \text{rad} = 0.875''$$

Einstein recovered exactly this value in 1911 by a completely different route — the equivalence principle alone, via gravitational time dilation. He pushed Freundlich to observe the 1914 eclipse in Crimea; the war interrupted the expedition. A historical stroke of luck: the prediction was wrong.

**Relativistic prediction (1915).** With the full theory, the **curvature of space** contributes as much as time dilation, and the result doubles:

$$\alpha = \frac{4GM}{c^2 b} = 1.75''$$

In the weak field, gravity acts on light like a medium of refractive index $n(r) = 1 + \dfrac{2GM}{rc^2}$. The deflection is the integral of the transverse component of the index gradient:

$$\alpha = \int |\nabla_\perp n|\,dz = \frac{2GM}{c^2}\int_{-\infty}^{+\infty}\frac{b}{(b^2+z^2)^{3/2}}\,dz = \frac{2GM}{c^2}\cdot\frac{2}{b} = \frac{4GM}{c^2 b}$$

The factor 4 splits into two equal halves: $2GM/(c^2b)$ from $g_{tt}$ (time), $2GM/(c^2b)$ from $g_{rr}$ (space). The 1911 theory, which ignored spatial curvature, saw only half of it.

The deflection varies as $1/b$: $1.75''$ at the limb, $0.87''$ at $2R_\odot$, $0.35''$ at $5R_\odot$. It is this **fall-off**, and not merely the value at the limb, that separates a genuine gravitational effect from an instrumental artefact.

**The measurement.** You photograph the star field around the eclipsed Sun, then the same field at night, months earlier or later, with the same instrument. The stars should appear displaced radially outwards. Plate scale, orientation and decentring must be fitted before extracting the deflection coefficient. The worst enemy is **thermal expansion**: the instrument heats up before the eclipse and cools abruptly during it.

**The 1919 results** (Dyson, Eddington, Davidson):

| Instrument | Result |
|---|---|
| Sobral, 4-inch refractor | $1.98'' \pm 0.12''$ |
| Príncipe, 13-inch | $1.61'' \pm 0.30''$ |
| Sobral, astrograph | $0.93''$ — **discarded** |

The third data set was rejected for thermal distortion of the coelostat. Because it happened to give almost the Newtonian value, Eddington was long suspected of cherry-picking his data. An independent re-analysis at Greenwich in 1979 confirmed that the rejection was technically justified.

**Today**, the deflection is written in the PPN formalism:

$$\alpha = \frac{1+\gamma}{2}\cdot\frac{4GM}{c^2 b}$$

with $\gamma = 1$ in general relativity and $\gamma = 0$ in the Newtonian case. All the physics sits in a single number. VLBI on quasars: $\gamma = 0.99992 \pm 0.00012$. Cassini, via the Shapiro delay: $\gamma - 1 = (2.1 \pm 2.3)\times 10^{-5}$ — $20\,000$ times sharper than 1919. Gaia is aiming at $10^{-6}$.

### Are eclipses still useful?

For helium and prominences, no — and they have not been for a long time. Janssen realised **the very day after** the 1868 eclipse that by greatly increasing dispersion you spread out the parasitic continuum while the emission lines stay concentrated: the contrast improves indefinitely. Then came Hale's spectroheliograph (1892), Lyot's coronagraph (1930) and the Lyot filter (1933), which made the observation a daily routine.

What remains irreplaceable is the **outer corona in white light between 1 and 3 solar radii**: there, coronagraphs are beaten by their own internal scattered light. Yet this is precisely the zone where the corona is heated and the solar wind accelerated. Modern campaigns do polarimetry there (mapping the coronal magnetic field) and fast spectroscopy.

## Examples & analogies

- **The shadow outruns an aircraft.** In the DSCOVR/EPIC videos (from the L1 Lagrange point, $1.5$ million km out, hence seeing the Earth's disc fully lit face-on), the lunar shadow crosses the globe at $1\,700$ km/h at the very least. That is why totality lasts only a few minutes at a given point. Geostationary satellites (Himawari, GOES, Meteosat) give the same show obliquely; the iconic photograph of 11 August 1999 over Europe was taken from Mir.
- **The contrast constraint** is the same one that stops you seeing stars in broad daylight: it is not that they are faint, it is that the background is too bright.

## Open questions

- The coronal heating mechanism remains open — see [[structure-of-the-sun.en]].
- Lunar eclipses and stellar occultations by the Moon (another way of measuring angular diameters) were not covered.
- The Saros cycle and eclipse prediction were not addressed.
- Modern gravitational lensing (Einstein rings, clusters, microlensing) is the direct descendant of the 1919 measurement and deserves its own sheet.

## Related notes

- [[spectroscopy-and-black-body.en]]
- [[structure-of-the-sun.en]]
- [[solar-wind.en]]
