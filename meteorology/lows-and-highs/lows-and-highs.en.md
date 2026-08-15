---
title: "Lows and Highs (Depressions and Anticyclones)"
aliases: []
tags: [meteorology, depression, anticyclone, coriolis, pressure]
created: 2026-06-28
source: conversation with Claude; web sources (NOAA, Wikipedia)
lang: en
translations:
  - "[[lows-and-highs.fr]]"
related:
  - "[[fronts-and-depression-clouds.en]]"
  - "[[general-atmospheric-circulation.en]]"
  - "[[jet-stream-and-rossby-waves.en]]"
  - "[[tropical-cyclones.en]]"
  - "[[nao-and-enso.en]]"
---

# Lows and Highs (Depressions and Anticyclones)

## TL;DR

> A low (depression) and a high (anticyclone) are two faces of the same physics: the pressure-gradient force combined with the Coriolis force makes the wind circle around the centres, and the associated vertical motion (ascent vs. subsidence) explains bad weather vs. fair weather.

## Key concepts

- **Surface pressure** — weight of the air column; sea-level average $\approx 1013\ \text{hPa}$.
- **Isobars** — lines of equal pressure; tightly packed = steep gradient = strong wind.
- **Coriolis force** — deflection due to the Earth's rotation: to the **right** in the northern hemisphere. Parameter $f = 2\Omega \sin\phi$ (zero at the equator, maximal at the poles).
- **Geostrophic wind** — balance between pressure and Coriolis: aloft, the wind blows **parallel** to the isobars.
- **Ascent / subsidence** — rising air (low) → clouds; sinking air (high) → clear skies.

## Deep dive

### The three forces that govern the wind

1. **Pressure-gradient force**: pushes air from high to low pressure, perpendicular to the isobars. Strength ∝ how tightly the isobars are packed.
2. **Coriolis force**: deflects motion to the right (N. hemisphere), $f = 2\Omega\sin\phi$. Zero at the equator — hence the absence of organised vortices there.
3. **Friction** (near the ground): slows the wind and makes it cut slightly across the isobars towards low pressure.

Aloft (no friction), pressure and Coriolis balance: a **geostrophic wind** parallel to the isobars. At the surface, friction breaks that balance → the wind spirals inwards in a low, outwards in a high.

### Direction of rotation (northern hemisphere)

- **Low**: anticlockwise, **convergent** (spiralling towards the centre).
- **High**: clockwise, **divergent** (spiralling outwards).

(Everything reverses in the southern hemisphere.) **Buys-Ballot's law**: with your back to the wind, the low is on your left.

### The vertical engine: why L = bad weather, H = fair weather

```
   (L) low pressure             (H) high pressure
   →→ converges at surface ←←   ←← diverges at surface →→
        ↑ ASCENT                     ↓ SUBSIDENCE
   expansion → cools            compression → warms
   → condensation               → evaporation
   → CLOUDS, RAIN               → CLEAR, DRY SKY
```

Air converging into a low has nowhere to go but up → it expands, cools, its moisture condenses → clouds and rain. In a high, air sinks (**subsidence**) → it compresses, warms, its moisture evaporates → settled weather. Subsidence often creates an **inversion** (a warm layer aloft) that traps moisture and pollutants: in winter a high also brings grey skies, fog and frost (clear nights). So "anticyclone" is not always a synonym for sunshine.

### The life cycle of a depression (Norwegian model)

Born on the **polar front** (the boundary between polar and subtropical air), often under a meander of the jet stream:

1. **Frontal wave** — an undulation forms on the polar front.
2. **Deepening (cyclogenesis)** — pressure falls, the rotation organises, a warm front + cold front + warm sector appear.
3. **Maturity** — the faster cold front catches up with the warm front.
4. **Occlusion** — the cold front joins the warm one and lifts the entire warm sector → **occluded front**; the depression peaks, then fills in (cut off from its warm air, it dies).

Typical duration: **3 to 7 days**. Depressions often arrive in **families** along the polar front.

## Examples & analogies

- **The bathtub**: a low = the plughole draining in a spiral (water turns, air rises → turbulence, clouds); a high = water poured in the middle and spreading out over the surface (dead calm). The direction of the spiral is imposed by Coriolis.
- **Tight isobars = storm**: on a weather map, the closer the lines around an "L", the more violent the wind — just as closely spaced contour lines mean a steep slope.

## Open questions

- Why is deepening favoured beneath certain parts of the jet? See [[jet-stream-and-rossby-waves.en]].
- Why do the "permanent" highs and lows sit at specific latitudes? See [[general-atmospheric-circulation.en]].

## Diagrams & videos

- Mid-latitude cyclones & anticyclones (illustrated course, PDF) — NWS/NOAA: <https://www.weather.gov/media/zhu/ZHU_Training_Page/Miscellaneous/cyclones_anticyclones/cyclones.pdf>
- Frontal depressions (model, diagrams) — Geosciences LibreTexts: <https://geo.libretexts.org/Bookshelves/Meteorology_and_Climate_Science/Atmospheric_Processes_and_Phenomena/12:_Extratropical_Cyclones/12.02:_Mid-latitude_Frontal_Cyclones>
- Video (FR) "Faire la pluie et le beau temps" — C'est pas sorcier: <https://www.youtube.com/watch?v=tAOCLReDoLs>

## Related notes

- [[fronts-and-depression-clouds.en]]
- [[general-atmospheric-circulation.en]]
- [[jet-stream-and-rossby-waves.en]]
- [[tropical-cyclones.en]]
- [[nao-and-enso.en]]
