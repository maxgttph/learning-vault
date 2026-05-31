---
title: "World Models"
aliases: ["world model", "modèle du monde", "model-based RL", "Dreamer", "JEPA"]
tags: [machine-learning, world-models, reinforcement-learning, model-based-rl, generative-models]
created: 2026-05-30
source: conversation with Claude
related: ["[[reinforcement-learning]]", "[[recurrent-networks-and-lstm]]", "[[embeddings-and-tokenization]]", "[[transformer-architecture]]", "[[neural-networks-overview]]", "[[how-neural-networks-train]]"]
---

# World Models

## TL;DR

> Un **world model** est un **simulateur appris** de la dynamique d'un environnement : il apprend, en auto-supervisé, la fonction $(s_t, a_t) \mapsto (s_{t+1}, r_t)$ — « si je fais cette action dans cet état, voici l'état et la récompense suivants ». Une fois ce simulateur acquis, un agent peut **s'entraîner dans son propre rêve** plutôt que dans le monde réel, ce qui économise énormément de données. C'est la brique centrale du [[reinforcement-learning|RL]] *model-based*.

## Key concepts

- **World model** — réseau génératif qui prédit l'état (et la récompense) suivants conditionnés par l'action ; sert de **simulateur** appris.
- **Model-based vs model-free** — model-based apprend d'abord la dynamique du monde puis s'en sert ; model-free apprend la politique directement, à l'aveugle (voir [[reinforcement-learning]]).
- **Espace latent** — espace de vecteurs appris, de basse dimension, où chaque point encode l'essence abstraite d'une observation, débarrassée de sa redondance.
- **V – M – C** — l'architecture canonique : **V**ision (compresse l'observation), **M**emory (modélise la dynamique dans le temps), **C**ontroller (la politique qui décide).
- **Entraînement dans le rêve** — apprendre la politique sur des trajectoires *imaginées* par le world model, sans toucher au monde réel.
- **Dérive (drift)** — l'erreur de prédiction s'accumule sur un long rêve auto-régressif ; le rêve est fiable à court horizon, fragile à long terme.

## Deep dive

### Le pivot de cadre : prédire le futur, conditionné par l'action

Les architectures « passives » (classifieur, [[transformer-architecture|Transformer]], [[recurrent-networks-and-lstm|RNN]]) prennent une entrée et produisent une sortie ; le modèle n'agit pas dans un monde qui lui répond. Un world model naît d'une question de [[reinforcement-learning|RL]] : un agent doit *agir*, et il existe deux écoles.

- **Model-free** (DQN, PPO) : apprend directement « dans cet état, quelle action paie le plus ? » par essais-erreurs. Aucune modélisation du monde — simple mais **vorace en données** (millions d'interactions réelles).
- **Model-based** : apprend d'abord un **modèle de la dynamique** $(s_t, a_t) \mapsto (s_{t+1}, r_t)$. C'est *ça*, un world model.

Ce qui distingue un world model d'un simple prédicteur de séquence, c'est l'**action en entrée** : il ne prédit pas la suite « naturelle » d'une séquence, mais la suite **conditionnée par ce que l'agent décide**. C'est ce qui le rend *contrôlable* et utile pour planifier ou rêver.

### L'espace latent (le « V »)

Une observation brute est énorme et redondante : une image $64 \times 64 \times 3 = 12\,288$ pixels. Mais l'information qui *compte* tient en quelques nombres (position, vitesse, courbure). Un encodeur apprend à projeter l'observation dans un **espace latent** de basse dimension (p. ex. $32$ nombres) :

$$ \underbrace{o_t}_{12\,288 \text{ pixels}} \;\xrightarrow{\;\text{encodeur}\;}\; \underbrace{z_t}_{32 \text{ nombres}} $$

Propriétés : compression (on jette le superflu, on garde les degrés de liberté), géométrie sémantique (deux situations proches → points proches), continuité (on peut interpoler). C'est la même idée que les [[embeddings-and-tokenization|embeddings]] du texte — un embedding *est* un vecteur latent — sauf qu'on compresse ici une entrée continue (image) et que le latent est réversible (un décodeur reconstruit l'observation).

### Comment on entraîne le « V » : le VAE

Le **V** est un **réseau de neurones** — en fait *deux* ([[neural-networks-overview|réseaux]] encodeur + décodeur), entraînés ensemble par descente de gradient ([[how-neural-networks-train]]).

**Point de départ — l'auto-encodeur simple.** Deux réseaux dos à dos autour d'un goulot :

```
image (12 288) ─► ENCODEUR ─► z (32) ─► DÉCODEUR ─► image reconstruite (12 288)
```

On l'entraîne en **auto-supervisé** — l'étiquette *est* l'entrée elle-même — pour minimiser l'erreur de reconstruction :

$$ \mathcal{L}_{\text{recon}} = \lVert\, o - \text{décodeur}(\text{encodeur}(o)) \,\rVert^2 $$

Pour bien reconstruire à travers un goulot de $32$ nombres, le réseau *doit* y comprimer l'essentiel. Le gradient remonte le décodeur puis l'encodeur, classiquement.

**Le « V » de Variational.** Un auto-encodeur simple suffit à comprimer, mais son espace latent est plein de **trous** : tire un $z$ au hasard et le décodeur produit de la bouillie — fatal pour un world model qui doit *rêver* des états plausibles. Le VAE corrige ça : l'encodeur ne produit pas un point $z$ mais les paramètres d'une **distribution** gaussienne, dont on échantillonne :

$$ \text{encodeur}(o) \rightarrow (\mu, \sigma), \qquad z \sim \mathcal{N}(\mu, \sigma^2) $$

La perte gagne un second terme — une régularisation (divergence de Kullback–Leibler) qui pousse ces gaussiennes vers $\mathcal{N}(0, I)$ :

$$ \mathcal{L}_{\text{VAE}} = \mathcal{L}_{\text{recon}} + \beta \cdot D_{\text{KL}}\big(\mathcal{N}(\mu,\sigma^2)\,\Vert\,\mathcal{N}(0,I)\big) $$

Les deux termes se tirent dessus (codes précis et éparpillés vs tout tassé autour de l'origine). Le compromis donne un latent **lisse, continu et sans trous** : un point tiré au hasard décode en observation plausible. **C'est précisément ce qui rend le rêve possible.**

**L'astuce de reparamétrisation.** Échantillonner $z \sim \mathcal{N}(\mu,\sigma^2)$ n'est pas dérivable, donc le gradient ne peut pas remonter vers l'encodeur. On sort le hasard du chemin :

$$ z = \mu + \sigma \odot \varepsilon, \qquad \varepsilon \sim \mathcal{N}(0, I) $$

Le bruit $\varepsilon$ est tiré à part, comme une entrée fixe ; $z$ devient une fonction dérivable de $\mu$ et $\sigma$, et la backprop traverse normalement.

### La dynamique (le « M »)

Le **M** modélise la dynamique dans le temps. C'est directement un [[recurrent-networks-and-lstm|RNN/LSTM]] : étant donnés le latent courant, l'action et l'état caché, il prédit le **prochain latent** et la **récompense** :

$$ (z_t, a_t, h_t) \;\xrightarrow{M}\; \big(\, P(z_{t+1}),\; \hat r_t,\; h_{t+1} \,\big) $$

C'est le schéma de récurrence habituel, mais (1) on injecte l'**action** en entrée et (2) on prédit l'**état du monde** suivant au lieu d'un token. Le fait que $M$ prédise aussi $\hat r_t$ est crucial : il rend le rêve **auto-suffisant** en récompense.

**Variante moderne : le Transformer.** Prédire $z_{t+1}$ à partir de l'historique $(z, a)$ *est* de la modélisation de séquence — un [[transformer-architecture|Transformer]] y excelle (entraînement parallélisable, dépendances longues). C'est ce que font IRIS, TransDreamer, Decision/Trajectory Transformer. Compromis usuel : le RNN/GRU est léger et streame (d'où son usage dans Dreamer) ; le Transformer coûte plus mais modélise des horizons plus longs.

### La politique (le « C ») et l'entraînement dans le rêve

Le **C** est souvent minuscule (linéaire) : il prend $[z_t, h_t]$ et sort l'action. Toute la connaissance du monde est dans $M$ ; $C$ n'a qu'à l'exploiter.

**On ne « conçoit » pas $C$ — on l'apprend.** On fixe seulement l'**objectif** (la récompense, p. ex. « survivre longtemps ») ; $C$ découvre tout seul la politique qui la maximise. Et il l'apprend **dans le rêve** : on déroule une partie entièrement dans $V+M$, sans le vrai jeu.

```
z_t,h_t ─► C ─► a_t ─► M ─► z_{t+1}, r̂_t, h_{t+1} ─► C ─► a_{t+1} ─► M ─► ...
        (décide)      (rêve la suite + la récompense)
```

La récompense ne vient pas du monde réel mais de la **prédiction $\hat r_t$ apprise par $M$**. On somme les récompenses imaginées = score de la politique, qu'on maximise (par évolution dans l'article de 2018, vu la petite taille de $C$ ; par gradient dans Dreamer). Des milliers de parties imaginaires, gratuites — c'est tout le gain d'efficacité-données.

### Le pipeline complet, et la nuance « pas à partir de rien »

1. **Collecte** — un agent au hasard interagit avec le vrai monde ; on enregistre les quadruplets $(o_t, a_t, o_{t+1}, r_t)$. Seul contact avec le monde réel ; auto-supervisé (le futur observé *est* l'étiquette, **aucun label humain**).
2. **Entraîner $V$** (image par image), puis encoder tout le log en séquences de latents.
3. **Entraîner $M$** sur ces séquences à prédire $z_{t+1}$ et $\hat r_t$.
4. **Entraîner $C$** dans le rêve généré par $V+M$.

Idée fausse à dissoudre : ce n'est **pas** « créer un dataset à partir de rien ». Le coût en données est **déplacé**, pas supprimé — on paie l'expérience réelle *une fois* pour apprendre le world model, puis la politique en profite à volonté. Et le rêve ne peut produire que ce qui est **dans l'enveloppe de ce qui a été appris** (jamais vu un mur exploser → jamais rêvé) ; les erreurs **dérivent** sur les longs horizons. D'où, dans les versions modernes (Dreamer), une **boucle** : collecter → améliorer $V,M$ → améliorer $C$ → recollecter avec un meilleur $C$ (qui atteint des zones nouvelles) → recommencer.

### À l'usage : déploiement

On rebranche le vrai environnement. Les **trois** réseaux tournent à chaque pas :

```
o_t (vraie image) ─V─► z_t ─► C(z_t, h_t) ─► a_t ─► [agir dans le vrai monde]
                          ▲                                  │
                          └── h_{t+1} ◄─ M(z_t, a_t, h_t)   o_{t+1} (vraie image)
```

Nuance importante : à l'usage, $M$ sert pour sa **mémoire $h_t$**, pas pour sa prédiction — on reçoit les vraies frames, donc plus besoin d'halluciner $z_{t+1}$ (c'était pour entraîner $C$). Si $C$ n'avait aucune mémoire ($C(z_t)$ seul), $V+C$ suffiraient ; mais dès qu'il faut se souvenir du passé, $M$ reste nécessaire.

### Les deux sens de « world model » (et le débat LeCun)

Le terme a deux acceptions, source de confusion :

- **Sens large** — tout modèle qui prédit l'étape suivante et encode une dynamique du monde. À ce titre, **un LLM est un world model du texte** : token = état, prédire le token suivant = la dynamique, choisir le token = l'action (le RLHF traite littéralement le LLM comme une politique). Le texte étant déjà discret et auto-étiquetant, le Transformer **fusionne $V$ et $M$** en un seul réseau.
- **Sens strict (Yann LeCun)** — un modèle qui prédit les *conséquences d'une action*, dans un *espace de représentation abstrait*, *ancré dans le sensoriel*, et *utilisable pour planifier*. Par cette définition, un LLM **n'est pas** un bon world model.

Le « paradoxe LeCun » (les LLM sont des world models *et* une impasse) se résout ainsi : c'est surtout un désaccord de **définition**, doublé d'une thèse de fond. Ses critiques de l'auto-régression générative sur le texte : (1) le texte est une projection appauvrie du réel (un bébé apprend la physique du sensoriel, avec moins de données et un meilleur modèle) ; (2) les erreurs auto-régressives **se composent exponentiellement** ; (3) reconstruire chaque détail (pixels, tokens) est le mauvais objectif. Sa proposition, **JEPA** (Joint Embedding Predictive Architecture, V-JEPA pour la vidéo) : prédire dans l'**espace abstrait des représentations**, **sans rien reconstruire** (non génératif), en gardant le prédictible et jetant l'imprévisible.

## Examples & analogies

- **Le moteur physique appris.** Un world model est comme le moteur physique d'un jeu vidéo — sauf qu'au lieu d'être codé à la main, il a été *appris* depuis des observations. Il ne décide rien ; il simule. La politique $C$ est le *joueur* qui s'entraîne dans ce moteur : il ne connaît pas la physique, il apprend à gagner en jouant des milliers de parties imaginaires.
- **CarRacing (article fondateur, Ha & Schmidhuber 2018).** Voiture vue de dessus : $o_t$ = image $64\times64$, $a_t$ = [direction, gaz, frein], $r_t$ = piste franchie. On collecte des parties aléatoires, on entraîne $V$ (VAE, $z$ à $32$ dims), puis $M$ (LSTM, $h$ à $256$ dims) sur les séquences de latents, puis $C$ (linéaire, $288 \to 3$) dans le rêve — sans jamais retoucher au vrai jeu pour cette dernière étape.

## Open questions

- **Extraire une formule de la loi apprise.** Le WM apprend une dynamique empirique en boîte noire. Peut-on en faire de la **régression symbolique** (AI Feynman, PySR) pour conjecturer une formule ? Faisable et actif, mais le latent est « emmêlé » : il faut le **désentrelacer** (disentangle) pour que ses dimensions correspondent à des grandeurs physiques lisibles.
- **Genie & les actions latentes.** Comment apprendre des actions *non supervisées* depuis de la vidéo brute (« quelle action expliquerait le passage de la frame $n$ à $n+1$ ? ») pour rendre contrôlable un monde appris sans labels d'action ?
- **Sora / GAIA-1 comme « world simulators ».** Jusqu'où un générateur de vidéo entraîné massivement constitue-t-il un world model utilisable pour planifier ?
- **JEPA vs auto-régression générative.** Le pari non génératif de LeCun va-t-il battre l'auto-régression scalée, ou l'inverse ?

## Related notes

- [[reinforcement-learning]]
- [[recurrent-networks-and-lstm]]
- [[embeddings-and-tokenization]]
- [[transformer-architecture]]
- [[neural-networks-overview]]
- [[how-neural-networks-train]]
