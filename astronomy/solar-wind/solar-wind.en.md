---
title: "The Solar Wind"
aliases: [Parker spiral, Coronal holes, Alfvén surface, Heliosphere, Heliopause]
tags: [astronomy, sun, plasma, heliosphere, magnetism]
created: 2026-08-11
source: conversation with Claude
lang: en
translations:
  - "[[solar-wind.fr]]"
related:
  - "[[structure-of-the-sun.en]]"
  - "[[solar-eclipses.en]]"
  - "[[nuclear-fusion-and-origin-of-matter.en]]"
---

# The Solar Wind

## TL;DR

> The solar corona, too hot to be held down by the Sun's gravity, pours continuously into space along open magnetic field lines, reaches $400$ to $800$ km/s within about twenty solar radii, then travels in a straight line out to $121$ AU. It is not an ejection: it is a **continuous leak**, uninterrupted for $4.6$ billion years.

## Key concepts

- **Solar wind** — a continuous, supersonic flow of plasma from the corona, filling the entire solar system.
- **Parker's argument** — no hydrostatic solution exists for the corona, so the outflow is compulsory.
- **Coronal hole** — a region where the magnetic field lines are open to space; the source of the fast wind.
- **Alfvén surface** — the boundary ($10$–$20\,R_\odot$) beyond which the plasma drags the field instead of being constrained by it.
- **Parker spiral** — the winding of the interplanetary magnetic field caused by solar rotation.
- **Freeze-in of ionisation** — the ionisation state locks in around $2$–$5\,R_\odot$ and keeps the signature of the source temperature forever.
- **Magnetic braking** — loss of angular momentum through the wind, which slows stellar rotation.
- **Heliopause** — the boundary of the solar domain, around $121$ AU.

## Deep dive

### It is not an ejection

Nothing is ejected. There is **no explosion, no jet, no expulsion**. The Sun's atmosphere escapes continuously because it is incapable of staying put. It has done so without a single second's interruption since the Sun formed.

Genuine ejections do exist — **coronal mass ejections** — but they are episodic events *superimposed* on the background wind. The wind itself always blows.

### Why the corona cannot stay

First instinct: "hot particles move fast enough to escape". That is **wrong**, and the gap is instructive:

- Escape velocity at the Sun's surface: $618$ km/s
- Thermal velocity of a proton at $10^6$ K: $\approx 128$ km/s

Protons are five times too slow. So this is not an evaporation of fast particles.

The mechanism is **collective and fluid**: the corona pushes on itself. Its pressure, very high for its density, exerts an outward gradient. Gravity falls off as $1/r^2$, while thermal conduction — remarkably efficient in a plasma — keeps the corona hot very far out, which stops its pressure from dropping as quickly. Beyond a few solar radii, **the pressure gradient beats gravity** and the gas is expelled wholesale.

**Parker's argument (1958)** is a proof by contradiction. Assuming the corona is in hydrostatic equilibrium, the calculation shows that at infinity its pressure would tend to a **finite, non-zero** value, far above that of the interstellar medium. That is physically impossible: the interstellar medium cannot contain such a pressure. The only consistent solution is that the corona **flows outward**, accelerating, and crosses the speed of sound within a few solar radii.

The paper was rejected twice by referees who judged the idea absurd. Chandrasekhar, editor of the *Astrophysical Journal*, overruled them. **Mariner 2**, en route to Venus in 1962, measured the flux directly.

The clues were there all along. Biermann had noticed in 1951 that comets' ion tails always point away from the Sun and react far too quickly for radiation pressure alone: a flow of **particles** was needed. And since Carrington (1859) and Chapman, it was known that geomagnetic storms followed solar flares with a one- or two-day delay — the travel time at a few hundred km/s.

### The journey, step by step

**Departure.** Everything hinges on the topology of the magnetic field. The corona is a landscape of magnetic bottles: where the field lines **loop back** to the Sun, plasma is trapped, accumulates and shines — those are the coronal loops; where they **run off to infinity**, nothing holds it back and it flows away. Those open regions are the **coronal holes**, dark in X-rays precisely because they are permanently draining. They occupy the poles at solar minimum and sometimes extend towards the equator.

**Acceleration ($0$ to $20\,R_\odot$).** The plasma crosses the **sonic point** around $5$–$10\,R_\odot$, then the **Alfvén surface** around $10$–$20\,R_\odot$. That second threshold is conceptually the more important: below it the magnetic field dominates and constrains the plasma, which is forced to co-rotate with the Sun; above it the plasma dominates and **drags the field along with it**. The umbilical cord is cut. Parker Solar Probe crossed it in April 2021 — the first spacecraft to "touch" the Sun. Most of the acceleration is over by $20\,R_\odot$.

**Cruise.** A near-radial trajectory, in a straight line, at constant speed. The trip to Earth takes **2 to 4 days**. Density falls as $1/r^2$ by simple geometric dilution.

**The end.** Around $85$–$95$ AU, the wind can no longer push back the interstellar medium: it decelerates abruptly at the **termination shock** (Voyager 1 in 2004, Voyager 2 in 2007), crosses the turbulent heliosheath, then reaches the **heliopause** around $121$ AU — Voyager 1 in August 2012, Voyager 2 in November 2018. These are the only two human objects in interstellar space.

### Composition and characteristics

By particle count: $\approx 96\%$ **protons**, $\approx 4\%$ **helium nuclei** ($\text{He}^{2+}$), traces of **heavy ions** (oxygen, carbon, iron, silicon, magnesium, neon, sulphur) at the $10^{-4}$ level, and exactly enough **electrons** for overall neutrality. Everything is ionised, without exception: not a single neutral atom.

| Quantity (at 1 AU) | Value |
|---|---|
| Speed — slow wind | $300$–$500$ km/s |
| Speed — fast wind | $700$–$800$ km/s |
| Density | $5$–$10$ particles/cm³ |
| Proton temperature | $\approx 10^5$ K |
| Magnetic field | $\approx 5$ nT |
| Sun's mass loss | $\approx 1.5\times 10^9$ kg/s |

That last figure sounds enormous (a million and a half tonnes per second) but, accumulated over $4.6$ Gyr, it amounts to **about $0.01\%$ of the Sun's mass**. Negligible — unlike massive stars, which can shed half their mass this way.

**A precious detail: the freeze-in of ionisation.** Around $2$–$5\,R_\odot$, the density becomes too low for recombination to keep happening, and the ionisation state **freezes**. The ions therefore carry the signature of the temperature they had at departure forever. By measuring the $\text{O}^{7+}/\text{O}^{6+}$ ratio at Earth, you read off directly the temperature of the coronal region of origin — three days and $150$ million kilometres earlier. **The wind carries the thermometer of its birthplace.** Combined with the FIP effect (see [[structure-of-the-sun.en]]), which enriches the slow wind in low-ionisation-potential elements, this makes it possible to trace the source region.

### What makes it strange

**It is unbelievably tenuous.** $5$ particles/cm³, against $2.5\times 10^{19}$ for air at sea level. To gather as much matter as there is in **one cubic millimetre of air**, you would have to rake up a cube of solar wind **$170$ metres on a side**.

**It is collisionless.** At Earth's distance, a proton's mean free path is of order $1$ AU: particles essentially never meet. And yet the whole behaves like a fluid, because the **magnetic field** couples them — each proton spirals around a field line with a gyroradius of some fifty kilometres. It is magnetism, not collisions, that provides the cohesion.

**It is not in thermal equilibrium.** Protons and electrons have different temperatures, and their velocity distributions are not Maxwellian — they show suprathermal tails.

**It is structured and recurrent.** The fast wind catches up with slow wind emitted earlier; the collision compresses the plasma into shells called **corotating interaction regions**. Since the Sun rotates, these structures sweep past Earth **every 27 days** — a very clean observational signature of solar rotation.

### Two winds, two origins

The **fast wind** emerges from coronal holes, where the plasma streams away unobstructed. It is steady, low-density, and dominates over the poles during solar minimum. **Ulysses** (1990–2009), the only probe ever to fly over the Sun's poles, mapped it.

The **slow wind** comes from the equatorial streamer belt and the edges of coronal holes, where field lines reconnect and reorganise. It is irregular, denser, and chemically enriched by the FIP effect.

### The Parker spiral

The Sun rotates in $\approx 25$ days while the plasma escapes radially. Since the magnetic field is "frozen" into the plasma yet stays anchored at the solar surface, it gets wound into an **Archimedean spiral**. At Earth's distance it makes an angle of $\approx 45°$ with the Sun-ward direction.

On top of this sits the **heliospheric current sheet**: a surface separating the Sun's two magnetic polarities, which the tilt of the magnetic axis warps into a "ballerina skirt". It is the largest coherent structure in the solar system.

### The wind brakes the Sun

Below the Alfvén surface, the plasma stays magnetically attached and is forced to co-rotate with the Sun. It therefore carries away **angular momentum** far more efficiently than its mass would suggest — the lever arm is the Alfvén radius, not the solar radius.

Consequence: stars slow down as they age. This is **magnetic braking**, regular enough to serve as a clock — **gyrochronology** dates a star from its rotation period. The Sun probably spun ten times faster in its youth.

### Its effects on us

The wind compresses the Earth's magnetic field on the dayside and stretches it into a long tail on the nightside: a bow shock at $\approx 15$ Earth radii, the magnetopause at $\approx 10$. Trapped particles produce the **auroras** by exciting oxygen (green at $557.7$ nm, red at $630$ nm) and nitrogen in the upper atmosphere.

**Coronal mass ejections** are far more violent than the steady wind. The Carrington event (1859) set telegraph lines on fire; the March 1989 one cut Quebec's electricity for nine hours; in February 2022 SpaceX lost 38 Starlink satellites because a minor storm had puffed up the upper atmosphere and increased drag.

But the wind also protects: the heliosphere it creates **excludes about $75\%$ of galactic cosmic rays**.

### What remains unexplained

The acceleration of the **fast** wind. Parker's thermal pressure accounts nicely for the slow wind, but does not reach $750$ km/s: extra momentum must be added along the way. Alfvén waves, turbulence, or the *switchbacks* — those hairpin reversals of the magnetic field discovered by Parker Solar Probe. It is the same knot of problems as coronal heating.

## Examples & analogies

- **The rocket nozzle.** Parker's solution is mathematically **identical** to that of a de Laval nozzle: a subsonic fluid accelerates, crosses the speed of sound at the throat, then keeps accelerating as it goes supersonic. In the solar case, the nozzle's role is played by gravity $+$ geometric expansion, and the "throat" sits around $5$–$10\,R_\odot$. A rocket engine whose nozzle is made of gravitational field.
- **The rotating sprinkler.** Every droplet leaves radially, yet the whole pattern draws a spiral because the source turns. That is exactly the Parker spiral — with the magnetic field playing the role of the jet.
- **Comet tails** always point away from the Sun, whatever the direction of the comet's motion. That is the clue that put Biermann on the trail in 1951, seven years before Parker.

## Open questions

- **The acceleration of the fast wind** is unexplained; *switchbacks* are a recent and poorly understood candidate.
- The link with **coronal heating** (see [[structure-of-the-sun.en]]) is probably the same problem seen from two angles.
- Space weather and the forecasting of coronal mass ejections would deserve their own sheet.
- Comparative magnetospheres (Earth, Jupiter, Mars with no global field) were not covered.
- The heliosphere / interstellar medium interaction beyond the heliopause remains poorly mapped.

## Related notes

- [[structure-of-the-sun.en]]
- [[solar-eclipses.en]]
- [[nuclear-fusion-and-origin-of-matter.en]]
