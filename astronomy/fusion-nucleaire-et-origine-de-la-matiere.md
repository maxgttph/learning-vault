---
title: "Fusion nucléaire stellaire et origine de la matière"
aliases: [Chaîne proton-proton, Deutérium, Défaut de masse, Nucléosynthèse primordiale, Cycle CNO]
tags: [astronomie, physique-nucleaire, cosmologie, soleil, nucleosynthese]
created: 2026-08-11
source: conversation with Claude
related:
  - "[[structure-du-soleil]]"
  - "[[spectroscopie-et-corps-noir]]"
  - "[[vent-solaire]]"
  - "[[circulation-atmospherique-generale]]"
---

# Fusion nucléaire stellaire et origine de la matière

## TL;DR

> La fusion n'allume pas une étoile — c'est la **gravité** qui la chauffe, et la fusion qui **arrête** l'effondrement. Aucune matière n'est éjectée par la fusion : quatre protons se lient en un noyau d'hélium $0{,}7\%$ plus léger, et cette masse manquante devient de l'énergie. L'hydrogène qui alimente tout cela date, lui, de la première microseconde de l'univers.

## Key concepts

- **Chaîne proton-proton** — la voie de fusion dominante dans le Soleil ($\approx 99\%$ de l'énergie) : $4\,{}^1\text{H} \rightarrow {}^4\text{He}$.
- **Deutérium** — isotope de l'hydrogène (1 proton + 1 neutron) ; seul état lié possible à deux nucléons.
- **Défaut de masse** — un système lié pèse *moins* que la somme de ses constituants ; la différence est l'énergie de liaison.
- **Goulot de l'interaction faible** — la première étape ($p+p$) exige la transmutation d'un proton en neutron, ce qui la ralentit d'un facteur astronomique.
- **Équilibre hydrostatique auto-régulé** — le thermostat qui rend une étoile stable pendant des milliards d'années.
- **Marche aléatoire des photons** — $\approx 10^5$ ans pour qu'un gamma du cœur ressorte, fragmenté en photons visibles.
- **Nucléosynthèse primordiale (BBN)** — les trois premières minutes, qui produisent $75\%$ H / $25\%$ He et rien de plus lourd.
- **Goulot du deutérium** — le blocage qui retarde la nucléosynthèse primordiale jusqu'à $t \approx 3$ min.

## Deep dive

### Le deutérium

C'est un **isotope de l'hydrogène** : même élément, même chimie, noyau plus lourd.

| | Noyau | Abondance terrestre | Stabilité |
|---|---|---|---|
| Protium (${}^1\text{H}$) | 1 proton | $99{,}98\%$ | stable |
| Deutérium (${}^2\text{H}$ ou D) | 1 proton $+$ 1 neutron | $0{,}0156\%$ | stable |
| Tritium (${}^3\text{H}$) | 1 proton $+$ 2 neutrons | traces | radioactif, $T_{1/2} = 12{,}3$ ans |

Sur Terre, environ **un atome de deutérium pour $6\,400$ d'hydrogène**, essentiellement dans les océans.

**Sa propriété nucléaire décisive.** Le deutéron est lié par seulement $2{,}22$ MeV — extraordinairement peu. Et surtout, **c'est le seul état lié possible à deux nucléons** : il n'existe ni « diproton » ni « dineutron ». Cette absence oblige la fusion stellaire à passer par l'interaction faible, ce qui la ralentit d'un facteur astronomique — et c'est cette lenteur qui donne au Soleil dix milliards d'années au lieu de quelques millions. Un univers où le diproton serait lié aurait des étoiles s'embrasant et s'éteignant en un clin d'œil ; il n'y aurait personne pour l'observer.

**Sa découverte** boucle joliment avec la spectroscopie. Urey, en 1931–1932, distille de l'hydrogène liquide puis regarde son spectre. Un noyau plus massif déplace légèrement les niveaux électroniques (effet de masse réduite : l'électron et le noyau tournent autour d'un centre de masse commun qui n'est pas au même endroit). La raie $\text{H}\alpha$ à $656{,}28$ nm est ainsi doublée d'une compagne à $656{,}10$ nm — un écart de $0{,}18$ nm. Prix Nobel de chimie 1934.

**Dans le Soleil**, le deutérium ne dure pas : un deutéron formé est consommé en $\approx 1$ seconde. C'est un intermédiaire fugace, jamais un réservoir. Le Soleil a d'ailleurs brûlé son stock initial *avant même de naître* : la fusion du deutérium s'amorce dès $10^6$ K, dix fois moins que celle de l'hydrogène ordinaire, donc toute proto-étoile en contraction la déclenche très tôt. C'est la définition de la frontière planète/étoile : au-dessous de **13 masses de Jupiter**, un objet n'atteint jamais le million de degrés ; au-dessus, il brûle son deutérium et devient une naine brune.

**Sa valeur cosmologique.** Le deutérium n'est fabriqué **nulle part** dans l'univers actuel — les étoiles ne peuvent que le détruire. Tout le deutérium existant date des trois premières minutes. Son abondance dépend très sensiblement de la densité de matière ordinaire au moment du Big Bang : c'est le meilleur « baryomètre » disponible.

### La chaîne proton-proton

Branche pp-I, $86\%$ de l'énergie solaire :

| Étape | Réaction | Énergie | Durée typique |
|---|---|---|---|
| 1 | $p + p \rightarrow \text{D} + e^+ + \nu_e$ | $1{,}44$ MeV | $\approx 10^9$ ans |
| 2 | $\text{D} + p \rightarrow {}^3\text{He} + \gamma$ | $5{,}49$ MeV | $\approx 1$ s |
| 3 | ${}^3\text{He} + {}^3\text{He} \rightarrow {}^4\text{He} + 2p$ | $12{,}86$ MeV | $\approx 400$ ans |

Les étapes 1 et 2 se produisent **deux fois** pour fournir les deux ${}^3\text{He}$ de l'étape 3. Bilan net :

$$4\,{}^1\text{H} \longrightarrow {}^4\text{He} + 2e^+ + 2\nu_e + 26{,}73\ \text{MeV}$$

Les positrons s'annihilent instantanément (leur contribution est déjà comptée). Les neutrinos emportent en moyenne $0{,}27$ MeV chacun et **quittent le Soleil en deux secondes** ; reste $\approx 26{,}2$ MeV déposés en chaleur.

**L'étape 1 est le goulot d'étranglement**, et l'écart des durées est de dix-sept ordres de grandeur. Deux protons qui se rencontrent ne peuvent pas rester ensemble puisque le diproton n'existe pas ; il faut qu'au moment précis du contact l'un des deux se transmute :

$$p \longrightarrow n + e^+ + \nu_e$$

C'est une désintégration bêta inverse, gouvernée par l'**interaction faible**. Elle est si improbable qu'un proton donné du cœur solaire attend en moyenne un milliard d'années, malgré $10^{14}$ collisions par seconde. Ce goulot règle tout : la longévité du Soleil, sa faible puissance volumique, et l'impossibilité de reproduire la chaîne pp en laboratoire — les réacteurs terrestres utilisent D $+$ T, qui ne passe pas par l'interaction faible.

**Les autres voies.** pp-II ($14\%$) passe par ${}^7\text{Be}$ et ${}^7\text{Li}$. pp-III ($0{,}02\%$) passe par ${}^8\text{B}$ et émet les **neutrinos de haute énergie** — les seuls que les premières expériences (Davis, Super-Kamiokande) savaient détecter ; c'est cette branche minuscule qui a révélé le problème des neutrinos solaires, puis leur oscillation. Le **cycle CNO** ($\approx 1\%$ dans le Soleil) utilise le carbone comme catalyseur : il capture successivement quatre protons et recrache un ${}^4\text{He}$ en se régénérant, pour un bilan identique. Il domine au-dessus de $\approx 1{,}3\,M_\odot$. Borexino en a détecté les neutrinos en 2020.

### Le défaut de masse : rien n'est éjecté

Les noyaux ne se séparent pas, ils **fusionnent** — et le produit **reste sur place**. L'hélium s'accumule au cœur depuis $4{,}6$ Gyr, faisant passer le noyau de $71\%$ à $34\%$ d'hydrogène. Aucune matière ne sort du Soleil par ce mécanisme.

Le compte de masse :

- 4 atomes d'hydrogène : $4 \times 1{,}007825 = 4{,}031300$ u
- 1 atome d'hélium-4 : $4{,}002602$ u
- Différence : $0{,}028698$ u, soit $\mathbf{0{,}7\%}$

Le noyau d'hélium est plus **léger** que les quatre protons dont il est issu. Cette masse n'est allée nulle part sous forme de particules : elle est devenue de l'énergie, selon $E = mc^2$.

> **La masse n'est pas conservée dans les réactions nucléaires — l'énergie l'est.** Un système lié pèse *moins* que la somme de ses constituants, parce que l'énergie de liaison est négative et compte dans la masse.

C'est vrai même en chimie : un atome d'hydrogène pèse un peu moins qu'un proton plus un électron séparés, de $13{,}6\ \text{eV}/c^2$. En nucléaire, l'effet est un million de fois plus grand et devient mesurable.

**Deux quantités à ne pas confondre :**

| Grandeur | Valeur | Fraction du Soleil |
|---|---|---|
| Hydrogène **transformé** en hélium depuis l'origine | $8{,}9\times 10^{28}$ kg | $\approx 4{,}5\%$ |
| Masse réellement **convertie en énergie** ($0{,}7\%$ du précédent) | $6{,}2\times 10^{26}$ kg | $\approx 0{,}03\%$ |

Les $4{,}5\%$ sont la masse *traitée*, pas perdue : elle est toujours là, sous forme d'hélium. Ce que le Soleil a réellement perdu est trente fois moins.

**Le débit actuel**, par seconde : $\approx 600$ millions de tonnes d'hydrogène fusionnées, $\approx 596$ millions de tonnes d'hélium produites, $\approx 4{,}3$ millions de tonnes volatilisées en énergie (soit exactement $L_\odot/c^2$). Curiosité : le Soleil perd **plus de masse par rayonnement** ($4{,}3$ Mt/s) que par le vent solaire ($1{,}5$ Mt/s). La lumière pèse.

### La gravité allume l'étoile, la fusion l'arrête

C'est la confusion la plus répandue sur les étoiles : la fusion **ne déclenche pas** le chauffage, elle l'interrompt.

1. **Un nuage moléculaire froid** ($10$–$20$ K), dilué. Rien ne s'y passe.
2. **L'effondrement gravitationnel**, déclenché par une perturbation — souffle de supernova, passage d'un bras spiral.
3. **La gravité chauffe.** C'est ici que naît la chaleur : l'énergie potentielle libérée par la chute devient agitation, donc température. Le théorème du viriel indique que la moitié est rayonnée, l'autre moitié reste en chaleur.
4. **À $10^6$ K**, le deutérium résiduel s'allume brièvement. Stock vite épuisé.
5. **À $\approx 10^7$ K**, la fusion de l'hydrogène s'amorce.
6. **La contraction s'arrête.** L'étoile se stabilise en équilibre hydrostatique et entre sur la séquence principale.

> **La fusion n'allume pas l'étoile — elle empêche l'étoile de continuer à s'effondrer.**

Sans elle, le Soleil brillerait quand même par contraction seule (Kelvin-Helmholtz), mais pendant $30$ millions d'années au lieu de $10$ milliards. La fusion n'est pas la source de la chaleur, c'est la source de la **durée**.

**Le thermostat.** Une fois installé, l'équilibre est auto-régulé : si la fusion s'emballe, le cœur chauffe, **se dilate**, se refroidit, et la fusion ralentit. Rétroaction négative avec une constante de temps de millions d'années. C'est pourquoi la luminosité ne varie que de $0{,}1\%$ sur un cycle de 11 ans, pour une puissance de $3{,}83\times 10^{26}$ W.

**Une surprise d'échelle.** À cause du goulot de l'interaction faible, la puissance dégagée par unité de volume au cœur du Soleil n'est que de $\approx 280$ W/m³, soit $\approx 0{,}002$ W/kg. Un corps humain produit $\approx 1{,}3$ W/kg, presque mille fois plus. **Le Soleil brille non parce qu'il est un réacteur intense, mais parce qu'il est énorme** — et c'est cette lenteur qui lui donne son autonomie.

### Le trajet de l'énergie, du cœur à la Terre

**a) Le cœur.** Photons gamma ($\approx 1$–$2$ MeV), positrons annihilés aussitôt, énergie cinétique thermalisée par collisions. Les neutrinos s'échappent immédiatement.

**b) La marche aléatoire — et la dégradation.** Le photon gamma parcourt $\approx 1$ mm avant d'être absorbé, puis réémis dans une direction quelconque. Il recommence des milliards de milliards de fois et met $\approx 10^5$ ans à dériver jusqu'à la surface. À chaque échange il se met en équilibre avec la température **locale**, qui baisse à mesure qu'il monte : un unique gamma de $1$ MeV finit converti en **$\approx 5\times 10^5$ photons visibles** de $2$ eV. L'énergie est conservée, mais fragmentée et dégradée. C'est pour cela que le Soleil brille en jaune et non en rayons gamma.

**c) La zone convective**, puis **la photosphère**, où le continuum planckien se forme et s'échappe — voir [[spectroscopie-et-corps-noir]] et [[structure-du-soleil]].

**d) $8$ min $20$ s.** Le contraste qui résume tout : les **neutrinos** traversés en ce moment ont été fabriqués il y a **8 minutes** ; les **photons** vus en ce moment ont été fabriqués il y a **$\approx 100\,000$ ans**. Ils sont issus de la même réaction.

**e) L'arrivée sur Terre.** Le rayonnement est le seul mécanisme possible : conduction et convection exigent de la matière, que le vide élimine. La luminosité $L_\odot = 3{,}83\times 10^{26}$ W, diluée sur une sphère de rayon $1$ UA :

$$\frac{L_\odot}{4\pi d^2} = \frac{3{,}83\times 10^{26}}{4\pi (1{,}496\times 10^{11})^2} = 1\,361\ \text{W/m}^2$$

C'est la constante solaire. La Terre en intercepte $\pi R_\oplus^2 = 1{,}27\times 10^{14}$ m², soit $1{,}74\times 10^{17}$ W — environ $10^4$ fois la consommation énergétique de l'humanité. Environ $30\%$ est réfléchi (albédo), $70\%$ absorbé.

À l'absorption, le photon excite un électron ou fait vibrer une molécule ; l'énergie se redistribue par collisions en quelques picosecondes en agitation désordonnée. Nuance de vocabulaire qui compte : le photon ne transporte pas de la chaleur, il transporte de l'**énergie**, qui devient chaleur en se thermalisant.

À l'équilibre, absorbé $=$ émis :

$$T = \left[\frac{S(1-A)}{4\sigma}\right]^{1/4} = 255\ \text{K} = -18\ ^\circ\text{C}$$

Les $33\ ^\circ$C manquants par rapport aux $+15\ ^\circ$C observés sont l'**effet de serre** : la lumière arrive dans le visible (pic à $0{,}50\ \mu$m par la loi de Wien pour $5\,772$ K) et traverse l'atmosphère facilement ; la Terre renvoie dans l'infrarouge lointain (pic à $\approx 11\ \mu$m pour $255$ K), où $\text{CO}_2$, vapeur d'eau et méthane absorbent. C'est ce chauffage différentiel selon la latitude qui met l'atmosphère en mouvement — voir [[circulation-atmospherique-generale]].

Le vent solaire apporte bien de la matière, mais son flux d'énergie est de $\approx 3\times 10^{-4}$ W/m², **dix millions de fois moins** que le rayonnement. Thermiquement nul ; son effet est magnétique.

### La suite du cycle stellaire

Dans $\approx 5$ Gyr, l'hydrogène du cœur sera épuisé. Le cœur, privé de sa source de pression, se contractera et chauffera — la gravité reprend la main, exactement comme au début. La fusion se déplacera en couche autour du cœur, l'enveloppe se dilatera énormément (**géante rouge**, jusqu'à engloutir Mercure et Vénus). Puis l'hélium s'allumera à $10^8$ K, brutalement (*flash de l'hélium*), produisant carbone et oxygène. Enfin l'enveloppe sera soufflée en **nébuleuse planétaire**, laissant un cœur de carbone dégénéré de la taille de la Terre : une **naine blanche**, qui refroidira pendant des milliers de milliards d'années.

Le Soleil est trop léger pour aller plus loin. Ni fer, ni supernova.

### D'où vient l'hydrogène

**$t < 10^{-6}$ s** — plasma de quarks et de gluons, trop chaud pour que protons et neutrons existent.

**$t \approx 10^{-6}$ s, $T \approx 10^{13}$ K** — l'expansion refroidit le milieu sous le seuil de confinement ; les quarks se lient par triplets. **Et un proton nu est déjà un noyau d'hydrogène.** L'hydrogène est le premier élément chimique, apparu avant toute étoile et toute galaxie.

**$t \approx 1$ s** — les neutrinos se découplent, l'interconversion cesse, le rapport se fige à $\approx 1$ neutron pour $6$ protons (asymétrie due à la masse un peu plus grande du neutron). Les neutrons libres se désintègrent ensuite avec $T_{1/2} = 10{,}2$ min ; à l'heure de la nucléosynthèse le rapport est descendu à $\approx 1{:}7$.

**$t \approx 3$ min, $T \approx 10^9$ K — le goulot du deutérium saute.** Pour construire l'hélium il faut passer par le deutérium ; or celui-ci est si faiblement lié ($2{,}22$ MeV) que tant que l'univers est trop chaud, chaque deutéron formé est pulvérisé par un photon. La nucléosynthèse est bloquée trois minutes, non par manque de matériau mais parce que l'**intermédiaire ne survit pas**. Vers $10^9$ K les photons sont enfin assez mous, le verrou saute, et tout s'enchaîne en quelques minutes.

**Le résultat, figé pour toujours** (en masse) :

| Espèce | Abondance primordiale |
|---|---|
| ${}^1\text{H}$ | $\approx 75\%$ |
| ${}^4\text{He}$ | $\approx 25\%$ |
| D | $\approx 10^{-5}$ |
| ${}^3\text{He}$ | $\approx 10^{-5}$ |
| ${}^7\text{Li}$ | $\approx 10^{-9}$ |
| tout le reste | **zéro** |

La synthèse s'arrête net pour deux raisons : il n'existe **aucun noyau stable de masse 5 ni de masse 8**, ce qui casse la chaîne d'assemblage ; et la densité chute si vite avec l'expansion que les réactions s'éteignent. Carbone, oxygène, fer — rien de tout cela n'existait encore. Il faudra les étoiles, et la triple collision de trois noyaux d'hélium, pour franchir ces trous.

**$t \approx 380\,000$ ans — la recombinaison.** Jusqu'ici les protons sont nus, baignés d'électrons libres. À $\approx 3\,000$ K les électrons se lient enfin : l'univers cesse d'être un plasma et devient **transparent**. Les photons libérés à cet instant sont exactement ceux du fond diffus cosmologique.

Donc, pour être précis : **les noyaux d'hydrogène ont $13{,}8$ milliards d'années moins une microseconde ; les atomes d'hydrogène en ont $13{,}8$ milliards moins $380\,000$ ans.**

**Comment on en est sûr.** La nucléosynthèse primordiale ne dispose que d'**un seul paramètre libre**, la densité de matière ordinaire, et prédit avec lui les abondances de D, ${}^3\text{He}$, ${}^4\text{He}$ et ${}^7\text{Li}$ — qui s'étalent sur **neuf ordres de grandeur**. Ce même paramètre se mesure indépendamment dans les pics acoustiques du fond diffus (Planck), et les deux valeurs coïncident : deux physiques sans rapport, l'une nucléaire à 3 minutes, l'autre acoustique à $380\,000$ ans, pointent le même nombre. Le deutérium, qu'aucune étoile ne fabrique, fournit la contrainte la plus fine en le mesurant dans des nuages intergalactiques vierges.

**Une réserve honnête** : le ${}^7\text{Li}$ observé est environ trois fois plus faible que prédit. Le « problème du lithium » n'est pas résolu.

**Comment cet hydrogène est arrivé dans le Soleil.** Il n'a presque rien fait pendant treize milliards d'années : le gaz s'est refroidi, rassemblé dans les puits de potentiel de la matière noire, a formé galaxies puis nuages moléculaires. Des générations d'étoiles s'y sont allumées et éteintes, n'en consommant qu'une petite fraction mais y injectant à chaque mort les éléments lourds forgés. Il y a $4{,}6$ Gyr, l'un de ces nuages contenait $1{,}3\%$ d'éléments lourds — la contribution accumulée de toutes les étoiles antérieures.

On sait même qu'une supernova a probablement déclenché l'effondrement : certaines météorites primitives contiennent les produits de désintégration d'**aluminium-26** ($T_{1/2} = 717\,000$ ans) et de **fer-60**. Ces noyaux ne survivent pas plus de quelques millions d'années — leur présence prouve qu'ils ont été injectés juste avant ou pendant la formation du système solaire.

## Examples & analogies

- **Une molécule d'eau réunit deux époques.** L'hydrogène s'est formé dans la première microseconde de l'univers et n'a jamais changé depuis. L'oxygène auquel il est lié a été fabriqué dans le cœur d'étoiles massives mortes il y a quelques milliards d'années. Deux atomes sur trois de chaque molécule d'eau du corps ont $13{,}8$ milliards d'années.
- **Le Soleil est un mauvais réacteur mais un gros réacteur.** $0{,}002$ W/kg — un tas de compost ou un corps humain font mieux au kilo. Toute sa puissance vient de son volume, et toute sa longévité de sa médiocrité par unité de volume.
- **Kelvin contre Darwin.** Kelvin, ne connaissant que la contraction gravitationnelle, calculait un Soleil de $30$ millions d'années et en concluait que l'évolution des espèces n'avait pas eu le temps de se produire. Sa physique était juste, son inventaire des sources d'énergie incomplet.

## Open questions

- Le **problème du lithium-7** (facteur 3 entre prédiction et observation) reste ouvert.
- La nucléosynthèse stellaire au-delà de l'hélium (processus triple-alpha, capture $\alpha$, processus $s$ et $r$) n'a pas été traitée et mériterait sa propre fiche.
- Les oscillations de neutrinos, seulement évoquées via le problème des neutrinos solaires.
- La fusion contrôlée sur Terre (D $+$ T, tokamaks, confinement inertiel) et pourquoi elle ne peut pas copier la chaîne pp.
- L'évolution stellaire détaillée après la séquence principale.

## Related notes

- [[structure-du-soleil]]
- [[spectroscopie-et-corps-noir]]
- [[vent-solaire]]
- [[circulation-atmospherique-generale]]
