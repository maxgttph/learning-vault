---
title: "Spectroscopie et corps noir : lire la lumière des étoiles"
aliases: [Raies de Fraunhofer, Lois de Kirchhoff, Équation de Saha, Corps noir, Raies telluriques]
tags: [astronomie, physique, spectroscopie, corps-noir, saha]
created: 2026-08-11
source: conversation with Claude
lang: fr
translations:
  - "[[spectroscopy-and-black-body.en]]"
related:
  - "[[solar-eclipses.fr]]"
  - "[[structure-of-the-sun.fr]]"
  - "[[nuclear-fusion-and-origin-of-matter.fr]]"
---

# Spectroscopie et corps noir : lire la lumière des étoiles

## TL;DR

> Un gaz dense et chaud émet un continuum qui ne dépend que de sa température (corps noir) ; les couches plus froides au-dessus y creusent des raies sombres aux longueurs d'onde propres à chaque élément. Toute la spectroscopie astronomique tient dans une idée : **chaque longueur d'onde te montre l'atmosphère stellaire à une altitude différente**, et l'altitude fixe la température, donc la brillance.

## Key concepts

- **Corps noir** — non pas une substance mais un *état* : matière assez dense pour thermaliser complètement le rayonnement, dont le spectre ne dépend alors que de $T$.
- **Lois de Kirchhoff** — dense chaud $\rightarrow$ continuum ; gaz raréfié sur fond noir $\rightarrow$ raies d'émission ; continuum vu à travers un gaz plus froid $\rightarrow$ raies d'absorption.
- **Loi de Kirchhoff du rayonnement thermique** — à chaque longueur d'onde, émissivité $=$ absorptivité : un bon absorbeur est un bon émetteur.
- **Profondeur optique $\tau$** — mesure de l'opacité cumulée ; $\tau = 1$ définit la « surface » visible à une longueur d'onde donnée.
- **Effet d'altitude** — une raie est sombre non parce qu'il manque de la lumière, mais parce qu'elle montre une couche plus haute et plus froide.
- **Équation de Saha** — donne le degré d'ionisation d'un gaz à l'équilibre, en fonction de $T$ **et** de la densité électronique.
- **Raies telluriques** — raies d'absorption produites par l'atmosphère terrestre, à distinguer des raies stellaires.
- **Spectre éclair** — inversion absorption $\rightarrow$ émission observée aux contacts d'une éclipse totale.

## Deep dive

### Pourquoi des raies existent

Dans un atome isolé, l'électron n'occupe que des **niveaux discrets** — les barreaux d'une échelle, pas une rampe. Passer de $E_1$ à $E_2$ exige exactement $E_2 - E_1$. Comme l'énergie d'un photon vaut $E = h\nu = hc/\lambda$, un atome ne peut interagir qu'avec des longueurs d'onde précises, fixées par l'espacement de ses barreaux, lui-même fixé par sa structure électronique. D'où l'empreinte digitale propre à chaque élément.

Kirchhoff et Bunsen l'établissent en 1859–1860 en montrant que les deux raies $D$ du Soleil tombent exactement là où le sodium chauffé en laboratoire émet les siennes.

### Pourquoi le corps noir n'a pas de raies

Dans la matière **dense**, les atomes sont collés, subissent des collisions permanentes et se perturbent mutuellement. Les niveaux, au lieu d'être des barreaux nets, s'élargissent jusqu'à fusionner en continuum. S'y ajoutent les électrons libres, qui n'étant liés à rien peuvent absorber ou émettre n'importe quelle énergie.

Trois mécanismes produisent effectivement ce continuum, et aucun ne fait intervenir de niveaux discrets :

- **libre-libre (bremsstrahlung)** — un électron libre dévié par un ion subit une accélération et rayonne ; pouvant perdre n'importe quelle fraction de son énergie cinétique, il émet à n'importe quelle longueur d'onde ;
- **libre-lié (recombinaison)** — un ion capture un électron libre ; le photon emporte l'énergie de liaison (fixe) *plus* l'énergie cinétique initiale de l'électron (quelconque) ;
- **l'ion $\text{H}^-$** — dominant dans le Soleil. Un hydrogène neutre peut retenir un second électron, très faiblement ($0{,}75$ eV). Tout photon d'énergie supérieure — soit $\lambda < 1{,}65\ \mu$m, ce qui couvre tout le visible — peut l'arracher, et le processus inverse émet du continuum. C'est $\text{H}^-$ qui rend la surface du Soleil opaque, donc qui produit la lumière qu'on voit. Compris seulement en 1939 (Wildt).

La forme exactement planckienne vient du **brassage** : dans un milieu optiquement épais, un photon est absorbé et réémis des milliards de fois avant de sortir, et à chaque échange il oublie son origine pour se conformer aux conditions locales. Le champ de rayonnement perd toute mémoire de la composition et ne dépend plus que de $T$.

Deux conséquences pratiques :

$$\lambda_{\max} = \frac{2898\ \mu\text{m}\cdot\text{K}}{T} \qquad\text{(Wien)} \qquad\qquad F = \sigma T^4 \qquad\text{(Stefan–Boltzmann)}$$

Le mot « noir » vient de la loi de Kirchhoff du rayonnement thermique : à chaque longueur d'onde, émissivité $=$ absorptivité. Le rayonneur thermique parfait est donc l'absorbeur parfait.

### Absorption : l'effet d'altitude

Le mécanisme immédiat est la **diffusion directionnelle**. Un photon à $589{,}0$ nm file vers la Terre ; un atome de sodium l'absorbe, puis réémet un photon identique dans une direction aléatoire sur $4\pi$ stéradians. Ce photon est perdu *pour nous*, pas pour l'univers. Rien n'est détruit.

Mais le vrai bilan est ailleurs, et c'est la reformulation qui débloque tout :

> **Une raie sombre n'est pas de la lumière manquante — c'est de la lumière venue d'une couche plus froide.**

À $500$ nm (hors raie), le gaz au-dessus de la photosphère est transparent : le regard plonge profond et s'arrête à $6\,400$ K. À $589{,}0$ nm (raie du sodium), ce même gaz est extrêmement opaque : le regard s'arrête beaucoup plus haut, à $4\,400$ K. La brillance variant comme $T^4$, le rapport est de $(6400/4400)^4 \approx 4{,}5$.

C'est pourquoi le fond des raies de Fraunhofer n'est **jamais noir** : il reste toujours un résidu, celui de la couche haute qui brille à sa propre température.

L'énergie « manquante » n'est pas perdue : elle est diffusée ailleurs, ou convertie par collisions et réémise à d'autres longueurs d'onde. Le blocage massif du flux par les raies métalliques dans le bleu réchauffe même les couches sous-jacentes (**backwarming**) et ressort décalé vers le rouge. Le flux total est rigoureusement conservé.

### La chaîne instrumentale

```
fente        → découpe une ligne fine (définit l'objet)
collimateur  → rend les rayons parallèles
réseau       → envoie chaque λ dans une direction différente
objectif     → convertit direction → position sur le détecteur
```

**La fente ne disperse pas** et, idéalement, ne diffracte pas : son rôle est géométrique. Un spectrographe forme une image de la source une fois par longueur d'onde, en décalant chaque image latéralement. Si la source est large, les images décalées se recouvrent et se moyennent en bouillie — c'est l'arc-en-ciel lisse d'un prisme tenu à la main. Si la source est une ligne fine, chaque longueur d'onde donne une ligne nette. **Une raie spectrale est littéralement une image de la fente** ; sa forme vient de la fente, pas de la physique atomique. L'analogie exacte est le sténopé, en une dimension.

La fente diffracte quand même, et c'est un défaut : trop étroite, elle étale le faisceau et dégrade la résolution tout en perdant de la lumière. Il existe une largeur optimale, typiquement quelques dizaines de $\mu$m.

**Le réseau disperse.** Ses sillons, espacés de $d$ (typiquement 600 à 1200 traits/mm, donc $d$ entre $1{,}7$ et $0{,}8\ \mu$m — comparable à $\lambda$, et c'est pour cela que ça marche), renvoient chacun la lumière dans toutes les directions. Les ondes issues de sillons voisins n'interfèrent constructivement que si leur différence de marche vaut un nombre entier de longueurs d'onde :

$$d\sin\theta = m\lambda$$

Chaque couleur ressort donc à son propre angle ; partout ailleurs les ondes s'annulent. L'objectif convertit ensuite angle en position : **sur le détecteur, la coordonnée horizontale est une longueur d'onde**.

Le pouvoir de résolution vaut $R = \lambda/\Delta\lambda = mN$ ($m$ l'ordre, $N$ le nombre de traits éclairés). L'ordre $m=0$ ne disperse pas ; les ordres élevés dispersent davantage mais se recouvrent. Les spectrographes **échelle** travaillent volontairement à $m \approx 50$–$100$ et ajoutent un disperseur croisé pour empiler les ordres : c'est l'architecture des instruments à $R \approx 10^5$, ceux qui détectent des exoplanètes par vitesse radiale.

Le Soleil étant très brillant, on peut se permettre $R \approx 10^6$ sans manquer de photons — luxe qu'on n'a pas sur les étoiles lointaines.

### L'équation de Saha

Elle donne la proportion d'atomes ionisés à l'équilibre thermique :

$$\frac{n_{i+1}\,n_e}{n_i} = 2\,\frac{g_{i+1}}{g_i}\left(\frac{2\pi m_e k T}{h^2}\right)^{3/2} e^{-\chi_i / kT}$$

Deux termes s'affrontent :

- **$e^{-\chi/kT}$** — le facteur de Boltzmann : probabilité qu'une collision fournisse l'énergie d'ionisation. Croît brutalement avec $T$.
- **$T^{3/2}/n_e$** — le terme d'**entropie** : un électron libre dispose d'un espace des états gigantesque comparé à un électron lié. D'où le résultat contre-intuitif : **l'ionisation est favorisée par la faible densité**, pas seulement par la chaleur.

Un chiffre rend la chose tangible. Dans la photosphère, $kT \approx 0{,}5$ eV alors que l'ionisation de l'hydrogène coûte $\chi = 13{,}6$ eV :

$$e^{-13{,}6/0{,}5} = e^{-27{,}2} \approx 1{,}5\times 10^{-12}$$

Or on mesure $\approx 10^{-4}$. L'écart de huit ordres de grandeur vient **entièrement du terme entropique** : avec $n_e \approx 10^{19}$ m⁻³, le préfacteur vaut $\approx 10^{8}$, et $10^{8}\times 1{,}5\times 10^{-12} \approx 1{,}6\times 10^{-4}$. C'est aussi pourquoi une atmosphère stellaire, très raréfiée, est bien plus ionisée qu'un gaz de laboratoire à la même température — et pourquoi la couronne atteint des états d'ionisation extravagants.

**Genèse.** Meghnad Saha, à Calcutta en 1920, fait un rapprochement que personne n'avait fait : l'ionisation est une **réaction chimique**, $A \rightleftharpoons A^+ + e^-$. Une fois écrite ainsi, la loi d'action de masse et l'isochore de van't Hoff s'appliquent. Il combine trois corpus qui ne se parlaient pas — thermodynamique chimique (Nernst, van't Hoff), mécanique statistique pour compter les états de l'électron libéré, et le modèle de Bohr tout récent qui fournit enfin les potentiels d'ionisation. Fowler et Milne la raffinent en 1923–1924 en proposant d'utiliser le **maximum d'intensité** d'une raie plutôt que son apparition ou sa disparition, critère bien plus objectif.

**Ce qu'elle a débloqué.** La séquence de Harvard O B A F G K M, établie empiriquement, devient une séquence de **température** et non de composition. Les raies de Balmer sont maximales dans les étoiles A ($\approx 9\,500$ K) : plus froid, trop peu d'atomes sont montés au niveau $n=2$ d'où part la série ; plus chaud, l'hydrogène est ionisé et n'a plus d'électron. L'hydrogène est pourtant le plus abondant partout.

Elle a aussi servi de thermomètre — c'est ainsi qu'Edlén a établi le million de degrés de la couronne — et son équivalent formel décrit la concentration de porteurs dans un semiconducteur, le raisonnement entropique étant identique.

**Sa limite.** Elle suppose l'**équilibre thermodynamique local** : tout gouverné par les collisions, à une seule température. Valable dans la photosphère et à l'intérieur des étoiles, elle tombe en panne dans la chromosphère et la couronne, où la densité est si faible que le champ de rayonnement — venu d'ailleurs, avec sa propre « température » — pilote l'ionisation plus que les collisions. Il faut alors résoudre l'équilibre statistique niveau par niveau (« hors ETL »).

### Force d'une raie $\neq$ abondance

C'est le piège central de l'analyse spectrale. Quatre facteurs interviennent en plus de la composition :

- **la température**, qui décide quels niveaux sont peuplés. L'hélium est l'exemple extrême : son premier niveau excité est à $19{,}8$ eV, le plus haut du tableau périodique. À $5\,800$ K, il n'y a essentiellement aucun atome d'hélium excité — l'hélium est **quasi invisible** dans le spectre photosphérique bien qu'il représente $25\%$ de la masse du Soleil ;
- **l'état d'ionisation** : Fe I, Fe II, … Fe XIV sont des systèmes atomiques *différents*, avec des raies différentes ;
- **la densité**, qui élargit les raies par collisions — c'est ainsi qu'on distingue une géante d'une naine ;
- **la vitesse** (Doppler) et le **champ magnétique** (Zeeman, qui dédouble les raies proportionnellement à $B$ — c'est ainsi que Hale a découvert en 1908 que les taches solaires portent $\approx 3\,000$ gauss).

Le contre-exemple historique est décisif. Vers 1920, on croyait les étoiles composées comme la Terre, parce que fer et calcium dominent visuellement le spectre solaire. **Cecilia Payne**, en 1925, applique Saha et démontre que la force d'une raie mesure le produit *abondance $\times$ facteur d'excitation*. Corrigé, le verdict tombe : l'hydrogène est $\approx 10^6$ fois plus abondant que les métaux en nombre d'atomes. Le résultat est si contraire au consensus qu'on lui fait ajouter qu'il est « certainement irréel ». Il était juste.

### Distinguer les raies solaires des raies terrestres

Les raies produites par notre propre atmosphère existent bel et bien : ce sont les **raies telluriques**. Cinq méthodes indépendantes les trient sans ambiguïté :

1. **La masse d'air.** Au ras de l'horizon, la lumière traverse jusqu'à 40 fois plus d'atmosphère : les raies telluriques s'approfondissent nettement, les raies solaires ne bougent pas. Test historique, mené par Janssen dans les années 1860 — c'est lui qui forge le mot « tellurique », et qui monte au sommet du Mont-Blanc pour minimiser la vapeur d'eau.
2. **Le décalage Doppler.** La Terre orbite à $30$ km/s : sur six mois, toutes les raies solaires se déplacent de $\pm 0{,}5$ Å tandis que les telluriques, produites par un gaz immobile par rapport au spectrographe, restent rigoureusement fixes. Variante élégante : le bord oriental du Soleil s'approche à $\approx 2$ km/s et le bord occidental s'éloigne, donc en déplaçant la fente d'un bord à l'autre du disque, les raies solaires oscillent et les telluriques ne bronchent pas.
3. **L'identité des espèces.** Les telluriques viennent de **molécules** — $\text{O}_2$, $\text{H}_2\text{O}$, $\text{CO}_2$, $\text{O}_3$ — qui seraient dissociées instantanément à $5\,800$ K. Leur signature est reconnaissable : ce sont des **bandes** (des amas de centaines de raies de rotation-vibration), pas des raies isolées.
4. **La comparaison entre objets.** Deux étoiles observées la même nuit dans la même direction montrent des telluriques identiques et des raies stellaires différentes.
5. **L'espace.** Au-dessus de l'atmosphère, elles disparaissent purement et simplement.

Détail historique savoureux : quand Fraunhofer étiquette ses raies de A à K en 1814, ses deux premières — la bande **A** à $759$ nm et la bande **B** à $686$ nm — sont de l'oxygène de notre propre atmosphère. Le catalogue fondateur de l'astrophysique commence par deux raies terrestres.

### Le spectre éclair : la démonstration

Avant le 2ᵉ contact d'une éclipse totale, le regard traverse la chromosphère et atteint la photosphère : **continuum brillant strié de raies sombres**. Après, la Lune ayant masqué la photosphère, la chromosphère se détache sur fond noir : **fond noir strié de raies brillantes**, aux longueurs d'onde exactement identiques.

> **Rien n'a changé dans le gaz. Seul le fond a changé.**

Les atomes émettaient déjà vers l'observateur avant l'éclipse ; simplement, leur émission propre était plus faible que le continuum qu'ils bloquaient au même endroit. Le solde était négatif. Retirer le continuum ne laisse que leur émission. Le « renversement » est une soustraction, pas un changement de physique — c'est la loi de Kirchhoff rendue visible en direct.

Kirchhoff avait **postulé** en 1859 que les raies de Fraunhofer naissent dans une couche relativement froide au-dessus de la source du continuum. Young le **voit** en 1870. Toute l'astrophysique stellaire repose sur cette validation.

Bonus observationnel : le renversement n'est pas simultané pour toutes les raies. Celles qui se forment très haut (hydrogène, hélium, H et K du calcium ionisé) flashent en premier et durent le plus longtemps. En filmant le spectre éclair à haute cadence, on **feuillette l'atmosphère solaire en altitude** — l'une des rares méthodes donnant directement la stratification verticale de la chromosphère.

## Examples & analogies

- **Le brouillard.** Un sol brûlant surmonté d'une couche de brouillard plus froide : on ne voit pas le sol, on voit le sommet du brouillard, qui brille moins. Le brouillard solaire est simplement **sélectif en longueur d'onde** — opaque dans les raies, transparent ailleurs. D'où l'effet d'altitude.
- **Le four du forgeron.** Un corps noir n'est pas invisible : froid, il paraît parfaitement noir (il ne réfléchit rien) ; chaud, il brille. Mais à l'**équilibre thermique** avec son environnement, émission et absorption se compensent exactement à chaque longueur d'onde et il devient rigoureusement indiscernable du fond. C'est le vécu quotidien des céramistes : dans un four à température uniforme, reliefs et contours disparaissent dans une soupe orange homogène ; il faut sortir la pièce pour la voir. Ce qui rend un objet visible est toujours un **écart**.
- **Le métal chauffé à blanc** est un thermomètre : $850$ K rouge sombre, $1\,300$ K rouge cerise, $1\,800$ K orange-jaune, $2\,800$ K blanc. Même « à blanc », le pic reste dans l'infrarouge — ce qu'on voit n'est que la queue de la courbe, devenue assez plate sur $0{,}4$–$0{,}7\ \mu$m pour que l'œil intègre en blanc. (Un métal réel n'est qu'un « corps gris » : émissivité $< 1$ et dépendant de $\lambda$, d'où la nécessité de calibrer les pyromètres.)

## Open questions

- Le transfert radiatif hors ETL (chromosphère, couronne) n'a été qu'effleuré.
- Les profils de raies détaillés — élargissement Doppler, collisionnel, ailes de Lorentz, courbe de croissance — mériteraient une fiche.
- Le problème des abondances solaires révisées (Asplund 2009), qui a cassé l'accord entre modèles et héliosismologie, reste non résolu.
- L'effet Zeeman et la magnétométrie solaire (spectropolarimétrie) n'ont pas été développés.

## Related notes

- [[solar-eclipses.fr]]
- [[structure-of-the-sun.fr]]
- [[nuclear-fusion-and-origin-of-matter.fr]]
