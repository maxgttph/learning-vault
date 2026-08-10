---
title: "Émulsions"
aliases: [Émulsifiants, Sauces émulsionnées]
tags: [chimie-culinaire, emulsion, tensioactif, physico-chimie, cuisine]
created: 2026-08-10
source: conversation with Claude
related:
  - "[[proteines-en-cuisine]]"
  - "[[epaississants-et-amidon]]"
  - "[[lipides-chaleur-et-aromes]]"
---

# Émulsions

## TL;DR

> Une émulsion est une dispersion forcée de gouttelettes de gras dans l'eau (ou l'inverse), maintenue par des molécules amphiphiles qui tapissent l'interface. Presque toutes sont **thermodynamiquement instables** : elles ne tiennent que parce que l'émulsifiant dresse une **barrière cinétique** contre la fusion des gouttelettes. La question n'est jamais « est-ce que ça va casser ? » mais « en combien de temps ? ».

## Key concepts

- **Émulsion** — dispersion d'un liquide en fines gouttelettes dans un autre liquide non miscible. « Huile-dans-eau » (mayonnaise, lait de coco) ou « eau-dans-huile » (beurre).
- **Effet hydrophobe** — l'huile et l'eau ne se repoussent pas : c'est l'eau qui préfère massivement se lier à elle-même. Phénomène majoritairement **entropique**.
- **Tension interfaciale $\gamma$** — coût énergétique par unité de surface de la frontière huile-eau. En $\text{J}\cdot\text{m}^{-2}$ (ou $\text{N}\cdot\text{m}^{-1}$).
- **Amphiphile / tensioactif** — molécule à tête hydrophile et queue(s) hydrophobe(s), qui s'adsorbe à l'interface et abaisse $\gamma$.
- **Coalescence** — fusion irréversible de deux gouttelettes dont le film protecteur a cédé. C'est le vrai « cassage ».
- **Crémage** — remontée (ou sédimentation) des gouttelettes par différence de densité, sans fusion. **Réversible.**

## Deep dive

### Pourquoi l'eau et l'huile s'ignorent

L'eau est **polaire** : l'oxygène, bien plus électronégatif que l'hydrogène, attire les électrons, créant une charge partielle négative sur $\text{O}$ et positive sur les $\text{H}$. La molécule étant coudée, ces charges ne s'annulent pas — l'eau porte un dipôle et tisse un réseau dense de liaisons hydrogène.

L'huile est **apolaire** : de longues chaînes hydrocarbonées où les liaisons $\text{C}\!-\!\text{H}$ sont électriquement quasi symétriques. Pas de dipôle, pas de liaison hydrogène — seulement de faibles forces de London.

Le point contre-intuitif : il n'y a pas de « répulsion » huile-eau. Ce qui domine est l'**effet hydrophobe**, une affaire d'entropie. Autour d'une goutte d'huile, les molécules d'eau ne peuvent plus former de liaisons hydrogène dans toutes les directions ; elles se réorganisent en une cage plus ordonnée, donc d'entropie basse. Le système minimise cette pénalité en réduisant au maximum la surface de contact. **Les gouttes fusionnent spontanément parce que moins de surface = moins d'eau contrainte.**

### La tension interfaciale, et pourquoi fouetter coûte de l'énergie

Le coût énergétique de créer de l'interface s'écrit :

$$\Delta G = \gamma \cdot \Delta A$$

Faire une émulsion, c'est déchirer l'huile en gouttelettes microscopiques, donc **augmenter $\Delta A$ de façon gigantesque** : $1\ \text{mL}$ d'huile fragmenté en gouttes de $1\ \mu\text{m}$ développe plusieurs $\text{m}^2$ d'interface. D'où l'énergie mécanique nécessaire (le fouet), et d'où la tendance spontanée du système à revenir en arrière — l'état séparé a moins d'interface, donc un $G$ plus bas.

L'émulsifiant agit sur l'autre terme : en s'intercalant à l'interface, il **abaisse $\gamma$**. Créer de l'interface devient beaucoup moins cher, et la défaire beaucoup moins urgent.

### L'émulsifiant : abaisser $\gamma$ et dresser une barrière

Une molécule amphiphile trouve à l'interface la seule position qui satisfasse ses deux extrémités : queue plongée dans l'huile, tête dans l'eau. En tapissant la gouttelette elle fait deux choses — abaisser $\gamma$, et empêcher physiquement la fusion. Deux mécanismes de stabilisation :

- **Répulsion électrostatique** — si la tête porte une charge (carboxylate $\text{–COO}^-$, phosphate), toutes les gouttes portent la même charge et se repoussent. Une double couche d'ions se forme autour de chacune ; leur chevauchement crée une barrière répulsive. *Corollaire : le sel, l'acide et le calcium, en écrantant ces charges, déstabilisent l'émulsion.*
- **Encombrement stérique** — les grosses molécules (protéines, polysaccharides) forment un matelas physique. Deux gouttes ne fusionnent pas parce qu'il y a littéralement de la matière entre elles.

Les meilleurs émulsifiants combinent les deux. La **lécithine** du jaune d'œuf en est l'archétype : un phospholipide à squelette glycérol, deux queues d'acides gras bien ancrées dans l'huile, et une tête phosphate-choline **zwitterionique** (charge $+$ et charge $-$). Deux queues pour l'ancrage, une tête chargée pour la répulsion.

Tous les émulsifiants ne sont pas des protéines — c'est l'erreur classique. Trois classes :

| Classe | Exemples | Mécanisme | Température |
|---|---|---|---|
| Petites molécules amphiphiles | Lécithine du jaune, mono-/diglycérides | Adsorption rapide, forte baisse de $\gamma$ | Efficace **à froid** |
| Protéines | Caséines, lipoprotéines du jaune, coco, arachide | Film viscoélastique, surtout **stérique** | Selon le type, voir [[proteines-en-cuisine]] |
| Polysaccharides (gommes) | Mucilage de moutarde, gomme arabique, xanthane | Encombrement + épaississement du milieu | À froid, stables à la chaleur |

La moutarde n'émulsionne donc pas grâce à ses protéines mais surtout grâce à ses **mucilages** — des polysaccharides issus de l'enveloppe de la graine. C'est un émulsifiant faible : d'où la fragilité structurelle d'une vinaigrette.

### L'idée centrale : instable, mais qui dure

Presque toutes les émulsions culinaires sont **thermodynamiquement instables**. L'état de plus basse énergie est toujours « huile d'un côté, eau de l'autre ». Ce qui les fait tenir n'est pas la thermodynamique mais la **cinétique** : l'émulsifiant élève une barrière si haute que la fusion devient extrêmement lente.

Une mayonnaise tient des heures non parce qu'elle est stable, mais parce que la barrière dressée par la lécithine est énorme. Une vinaigrette casse en trois minutes parce que celle de la moutarde est basse. *(Exception rare : les **microémulsions**, à gouttelettes nanométriques, réellement stables et de formation spontanée.)*

### Les quatre morts d'une émulsion

```
état dispersé
     │
     ├─► CRÉMAGE / SÉDIMENTATION   gouttes intactes, triées par densité   RÉVERSIBLE
     │
     ├─► FLOCULATION               amas de gouttes, films intacts         RÉVERSIBLE
     │
     ├─► COALESCENCE               films rompus, 2 gouttes → 1            IRRÉVERSIBLE
     │
     └─► MÛRISSEMENT D'OSTWALD     les grosses grossissent aux dépens
                                   des petites (pression de Laplace)      IRRÉVERSIBLE
```

Le mûrissement d'Ostwald est le plus subtil : la pression interne d'une gouttelette suit la loi de Laplace, $\Delta P = 2\gamma / r$. Les petites gouttes, à forte pression interne, voient leurs molécules d'huile diffuser lentement à travers la phase aqueuse vers les grosses. Lent, mais sans retour.

### Trois cas pratiques, trois diagnostics

**Vinaigrette qui retombe.** Émulsion née fragile : la moutarde ne suffit pas à couvrir toutes les gouttelettes. À l'arrêt du fouet, la mousse d'air s'effondre (drainage des films, éclatement des bulles) puis l'émulsion crème et coalesce. Les « petites bulles agglomérées » observées sont de l'huile qui se recolle ; le « jus » est la phase aqueuse plus dense qui coule dessous. **Problème de construction.**

**Mafé, curry coco.** Ici l'émulsion préexiste et elle est robuste : le lait de coco est une émulsion huile-dans-eau stabilisée d'usine par les protéines de coco ; le beurre de cacahuète est une suspension de particules broyées dans l'huile d'arachide. Le coupable est **thermique** — dénaturation puis agrégation des protéines qui tapissaient les gouttelettes (le film tombe), évaporation de l'eau qui déséquilibre le ratio gras/eau, fluidification de la phase aqueuse qui accélère les collisions, et ébullition turbulente qui déchire mécaniquement les films. **Problème de conservation sous contrainte thermique.** Voir [[proteines-en-cuisine]].

**Mayonnaise qui grène.** Pas de chaleur, donc pas de coagulation protéique. Ce sont soit des gouttelettes coalescées en amas (huile ajoutée trop vite : localement plus d'huile que la lécithine n'en peut tapisser), soit des **cristaux de triglycérides** si l'huile ou la mayonnaise a pris froid. Dans les deux cas c'est physique.

> **Le test de réversibilité est le meilleur diagnostic.** Si ça repart au fouet, c'était de la physique (du gras). Si ça reste grumeleux quoi qu'on fasse, c'était de la chimie (des protéines coagulées).

### Construire, et rattraper

Pour qu'une émulsion tienne : plus d'émulsifiant ; huile versée **en filet mince** sous fouettage continu (sinon on dépasse la capacité d'encapsulation) ; ratio phase aqueuse / émulsifiant / huile respecté ; et si la tenue doit être longue, un émulsifiant fort (jaune d'œuf) ou un épaississant qui fige les gouttelettes en place (voir [[epaississants-et-amidon]]).

Pour rattraper : refouetter suffit souvent si l'on en est au crémage. Si c'est vraiment déphasé, utiliser la **méthode de l'amorce** — dans un bol propre, une cuillère de moutarde fraîche (ou d'eau tiède, ou un jaune), et y verser la sauce cassée en filet, en fouettant. On reconstruit l'émulsion autour d'une base d'émulsifiant neuf.

## Examples & analogies

- **Le fouet paie une facture de surface.** Émulsionner, c'est acheter de l'interface avec de l'énergie mécanique, à un prix unitaire $\gamma$. L'émulsifiant est la remise commerciale : il fait baisser le prix au mètre carré, et poste un vigile devant chaque gouttelette.
- **La vinaigrette et la mayonnaise sont le même objet à deux échelles de temps.** Rien ne les distingue conceptuellement — seulement la hauteur de la barrière cinétique : trois minutes contre toute une soirée.
- **La séparation est parfois la technique, pas le raté.** En cuisine thaïe on fait bouillir fort la crème de coco *exprès* pour la casser (« crack the coconut cream ») et faire revenir la pâte de curry dans l'huile de coco rendue. Un mafé abouti présente presque toujours sa nappe d'huile orangée. La cible n'est pas « zéro séparation » mais **séparation maîtrisée**.

## Open questions

- Le **HLB** (Hydrophilic-Lipophilic Balance) : l'échelle qui classe les émulsifiants selon l'équilibre entre leurs parties hydrophile et lipophile — et qui prédit si l'on obtiendra une émulsion huile-dans-eau ou eau-dans-huile.
- Pourquoi certaines émulsions sont-elles **inverses** (eau-dans-huile : beurre, margarine) ? Règle de Bancroft.
- Les émulsions stabilisées par des **particules solides** (émulsions de Pickering) — ni tensioactif, ni protéine.

## Related notes

- [[proteines-en-cuisine]]
- [[epaississants-et-amidon]]
- [[lipides-chaleur-et-aromes]]
