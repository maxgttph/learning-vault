---
title: "Comment fonctionne un neurone"
aliases: []
tags: [neuroscience, neurone, electrophysiologie, potentiel-action]
created: 2026-06-20
source: conversation with Claude
lang: fr
translations:
  - "[[neuron-function.en]]"
related: ["[[cortical-hyperexcitability.fr]]", "[[migraine-pathophysiology.fr]]"]
---

# Comment fonctionne un neurone

## TL;DR

> Un neurone est une cellule qui reçoit, intègre et transmet des signaux électriques. Il fonctionne en maintenant un gradient ionique de part et d'autre de sa membrane (une « pile » chargée), en déchargeant un **potentiel d'action** tout-ou-rien lorsque la stimulation franchit un seuil, et en passant chimiquement le message au neurone suivant au niveau d'une **synapse**. Une pompe gourmande en énergie recharge la pile en permanence.

## Key concepts

- **Potentiel de repos** — l'état chargé mais inactif du neurone : intérieur négatif (~$-70$ mV) par rapport à l'extérieur.
- **Canaux ioniques** — tunnels protéiques sélectifs pour un seul ion (Na⁺, K⁺, Ca²⁺) ; soit *voltage-dépendants* (s'ouvrent sur un changement de tension), soit *ligand-dépendants* (s'ouvrent quand un neurotransmetteur se fixe).
- **Seuil** — la tension membranaire (~$-55$ mV) au-delà de laquelle la décharge devient inévitable. L'idée la plus importante pour comprendre l'excitabilité.
- **Potentiel d'action** — la décharge électrique tout-ou-rien qui parcourt l'axone.
- **Synapse** — l'espace où le signal est transmis chimiquement au neurone suivant.
- **Neurotransmetteurs** — messagers chimiques ; le **glutamate** excite, le **GABA** inhibe.
- **Pompe Na⁺/K⁺** — le « nettoyeur » consommateur d'ATP qui restaure le gradient après la décharge.

## Deep dive

### Anatomie

Un neurone comporte trois parties fonctionnelles : les **dendrites** (les antennes qui reçoivent les signaux des autres neurones), le **corps cellulaire / soma** (qui intègre le tout) et l'**axone** (le câble qui porte la sortie vers ses terminaisons puis vers les neurones suivants).

### Le potentiel de repos — une pile chargée

Au repos, le neurone est **polarisé** : l'intérieur est à environ $-70$ mV par rapport à l'extérieur. Cet état est entretenu par une répartition inégale des ions de part et d'autre de la membrane — beaucoup de **sodium ($\text{Na}^+$)** et de **calcium ($\text{Ca}^{2+}$)** dehors, beaucoup de **potassium ($\text{K}^+$)** dedans. Comme une pile chargée, il est prêt à se décharger.

### Les canaux ioniques — des portes sélectives

La membrane est criblée de **canaux ioniques**, des tunnels protéiques laissant passer chacun un ion précis. Les canaux *voltage-dépendants* s'ouvrent quand la tension membranaire change ; les canaux *ligand-dépendants* s'ouvrent quand un neurotransmetteur s'y fixe.

### Le potentiel d'action — tout ou rien

1. Les signaux excitateurs entrants dépolarisent légèrement la membrane (la rendent moins négative).
2. Si elle atteint le **seuil** (~$-55$ mV), les canaux $\text{Na}^+$ voltage-dépendants s'ouvrent en masse → le $\text{Na}^+$ se précipite à l'intérieur → l'intérieur devient positif. C'est le **potentiel d'action** : une pointe rapide et **tout-ou-rien** — soit elle part complètement, soit elle ne part pas.
3. Les canaux $\text{K}^+$ s'ouvrent ensuite → le $\text{K}^+$ sort → la membrane redevient négative (**repolarisation**).
4. La pointe se propage le long de l'axone jusqu'aux terminaisons.

À retenir : tout ce qui rapproche la membrane du seuil rend le neurone **plus facile à déclencher** — donc plus excitable.

### La synapse — passer le message

À la terminaison de l'axone, le signal électrique doit franchir un espace minuscule (la **synapse**) :

1. Le potentiel d'action ouvre des canaux $\text{Ca}^{2+}$ → le calcium entre.
2. Le calcium déclenche la libération de **neurotransmetteurs** dans la synapse.
3. Ils se fixent aux récepteurs du neurone suivant :
   - **Glutamate** → ouvre des canaux laissant entrer $\text{Na}^+/\text{Ca}^{2+}$ → **excite** (pousse vers le seuil).
   - **GABA** → ouvre des canaux laissant entrer $\text{Cl}^-$ (ou sortir $\text{K}^+$) → **inhibe** (éloigne du seuil).

Chaque neurone additionne en permanence des milliers d'entrées excitatrices (glutamate) et inhibitrices (GABA) ; il ne décharge que si la somme franchit le seuil.

### La pompe Na⁺/K⁺ — le nettoyeur gourmand en énergie

Après une décharge, les ions sont mal placés (le $\text{Na}^+$ est entré, le $\text{K}^+$ est sorti). La **pompe $\text{Na}^+/\text{K}^+$** rétablit l'ordre — en éjectant le $\text{Na}^+$, en ramenant le $\text{K}^+$ — au prix de l'**ATP** (l'énergie cellulaire produite par les mitochondries). C'est le point faible du système : quand l'apport énergétique flanche, la pompe est débordée et le gradient se dégrade.

## Examples & analogies

- **La pile chargée** : le potentiel de repos stocke une énergie prête à être libérée ; la pompe la maintient chargée.
- **Une gâchette avec un point de déclenchement** : la décharge est tout-ou-rien passé le seuil — comme la détente d'une arme, pas comme un variateur. Abaisser le seuil, c'est exactement ce que signifie une excitabilité « à fleur de peau ».

## Open questions

- Comment la myéline et la conduction saltatoire accélèrent la propagation le long de l'axone.
- Comment les cellules gliales (astrocytes) tamponnent le $\text{K}^+$ et le glutamate extracellulaires.

## Related notes

- [[cortical-hyperexcitability.fr]]
- [[migraine-pathophysiology.fr]]
