---
title: "Dépressions et anticyclones"
aliases: []
tags: [meteorologie, depression, anticyclone, coriolis, pression]
created: 2026-06-28
source: conversation with Claude; compléments web (NOAA, Wikipedia)
related:
  - "[[fronts-et-nuages-des-depressions]]"
  - "[[circulation-atmospherique-generale]]"
  - "[[jet-stream-et-ondes-de-rossby]]"
  - "[[cyclones-tropicaux]]"
  - "[[nao-et-enso]]"
---

# Dépressions et anticyclones

## TL;DR

> Dépression (basse pression) et anticyclone (haute pression) sont les deux faces d'une même physique : la combinaison force de pression + force de Coriolis fait tourner le vent autour des centres, et le mouvement vertical associé (ascendance vs subsidence) explique mauvais temps vs beau temps.

## Key concepts

- **Pression au sol** — poids de la colonne d'air ; moyenne au niveau de la mer $\approx 1013\ \text{hPa}$.
- **Isobares** — courbes de même pression ; serrées = fort gradient = vent fort.
- **Force de Coriolis** — déviation due à la rotation terrestre : vers la **droite** dans l'hémisphère Nord. Paramètre $f = 2\Omega \sin\phi$ (nul à l'équateur, maximal aux pôles).
- **Vent géostrophique** — équilibre pression ↔ Coriolis : le vent souffle **parallèlement** aux isobares en altitude.
- **Ascendance / subsidence** — montée d'air (dépression) → nuages ; descente (anticyclone) → ciel clair.

## Deep dive

### Les trois forces qui gouvernent le vent

1. **Force de gradient de pression** : pousse l'air de la haute vers la basse pression, perpendiculairement aux isobares. Intensité ∝ resserrement des isobares.
2. **Force de Coriolis** : dévie le mouvement vers la droite (hém. N), $f = 2\Omega\sin\phi$. Nulle à l'équateur — d'où l'absence de tourbillons organisés là-bas.
3. **Frottement** (près du sol) : freine le vent et le fait traverser un peu les isobares vers la basse pression.

En altitude (sans frottement), pression et Coriolis s'équilibrent : **vent géostrophique** parallèle aux isobares. Au sol, le frottement casse l'équilibre → le vent spirale vers l'intérieur d'une dépression, vers l'extérieur d'un anticyclone.

### Sens de rotation (hémisphère Nord)

- **Dépression** : antihoraire, **convergent** (spirale vers le centre).
- **Anticyclone** : horaire, **divergent** (spirale vers l'extérieur).

(Tout s'inverse dans l'hémisphère Sud.) **Loi de Buys-Ballot** : dos au vent → dépression à gauche.

### Le moteur vertical : pourquoi D = mauvais temps, A = beau temps

```
   (D) basse pression          (A) haute pression
   →→ converge au sol ←←        ←← diverge au sol →→
        ↑ ASCENDANCE                ↓ SUBSIDENCE
   détente → refroidit          compression → réchauffe
   → condensation               → évaporation
   → NUAGES, PLUIE              → CIEL CLAIR, SEC
```

L'air qui converge dans une dépression ne peut que monter → se détend, refroidit, l'humidité condense → nuages et pluie. Dans un anticyclone l'air descend (**subsidence**) → se comprime, se réchauffe, l'humidité s'évapore → temps stable. La subsidence crée souvent une **inversion** (couche chaude en altitude) qui piège humidité et polluants : en hiver l'anticyclone donne aussi grisaille, brouillards, gelées (ciel clair la nuit). « Anticyclone » n'est donc pas toujours synonyme de soleil.

### Le cycle de vie d'une dépression (modèle norvégien)

Naissance sur le **front polaire** (frontière air polaire/air subtropical), souvent sous un méandre du jet-stream :

1. **Onde frontale** — une ondulation se forme sur le front polaire.
2. **Creusement (cyclogenèse)** — la pression chute, la rotation s'organise, front chaud + front froid + secteur chaud apparaissent.
3. **Maturité** — le front froid, plus rapide, rattrape le front chaud.
4. **Occlusion** — le front froid rejoint le chaud, soulève tout le secteur chaud → **front occlus** ; la dépression atteint son maximum puis se comble (coupée de son air chaud, elle meurt).

Durée typique : **3 à 7 jours**. Les dépressions arrivent souvent **en familles** le long du front polaire.

## Examples & analogies

- **La baignoire** : la dépression = bonde qui se vide en spirale (l'eau tourne, l'air monte → agitation, nuages) ; l'anticyclone = eau versée au centre qui s'étale en surface (calme plat). Le sens de la spirale est imposé par Coriolis.
- **Isobares serrées = tempête** : sur une carte météo, plus les lignes sont rapprochées autour d'un « L », plus le vent sera violent — comme des courbes de niveau rapprochées = pente raide.

## Open questions

- Pourquoi le creusement est-il favorisé sous certaines parties du jet ? Voir [[jet-stream-et-ondes-de-rossby]].
- Pourquoi les anticyclones et dépressions « permanents » siègent-ils à des latitudes précises ? Voir [[circulation-atmospherique-generale]].

## Schémas & vidéos

- Cyclones & anticyclones des latitudes moyennes (cours illustré, PDF) — NWS/NOAA : <https://www.weather.gov/media/zhu/ZHU_Training_Page/Miscellaneous/cyclones_anticyclones/cyclones.pdf>
- Dépressions frontales (modèle, schémas) — Geosciences LibreTexts : <https://geo.libretexts.org/Bookshelves/Meteorology_and_Climate_Science/Atmospheric_Processes_and_Phenomena/12:_Extratropical_Cyclones/12.02:_Mid-latitude_Frontal_Cyclones>
- Vidéo (FR) « Faire la pluie et le beau temps » — C'est pas sorcier : <https://www.youtube.com/watch?v=tAOCLReDoLs>

## Related notes

- [[fronts-et-nuages-des-depressions]]
- [[circulation-atmospherique-generale]]
- [[jet-stream-et-ondes-de-rossby]]
- [[cyclones-tropicaux]]
- [[nao-et-enso]]
