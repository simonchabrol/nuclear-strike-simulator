# Nuclear Strike Simulator

## English

### Overview

This simulator is a direct visual implementation of the casualty model developed in the article *"The consequences of a nuclear war: case study on 80s UK"* (Simon Chabrol, 2025), itself based on the British government's **Square Leg** civil defence exercise (1980).

The tool allows any user to select one of 200 pre-loaded cities — or enter a city manually — and visualise the estimated human consequences of a nuclear strike using the same formulas a military planner would apply.

### How it works

The simulation is built around two formulas derived from the article.

The **casualties formula** computes a weighted death rate across the two urban zones that make up any modern city. The core city zone — the densest, most urbanised part — is assigned an 85% death rate, reflecting the near-total destruction caused by a direct airburst over a compact urban area. The wider metropolitan zone receives a 50% death rate, accounting for the more dispersed population and the decreasing intensity of the blast wave at greater distances. The weighted average of these two rates, applied to the total population, gives the estimated death count.

```
Casualties = ( (C × DC) + (M × DM) ) / 100

  C  = percentage of the population living in the core city
  M  = percentage of the population living in the metropolitan area (C + M = 100)
  DC = 85  (core city death rate, in %)
  DM = 50  (metropolitan area death rate, in %)
```

The **megatons formula** then estimates the explosive power required to produce those casualties, based on the empirical relationship between a 1.5 MT weapon and an average of 344,000 deaths — a figure validated against NUKEMAP across multiple cities in the article.

```
Megatons = (Deaths / 344,000) × AvgMT

  AvgMT = 1.5 (average yield per bomb, adjustable in the interface)
```

### The visualisation

The 50×50 dot grid represents the entire population of the city. Each dot is a proportional fraction of that population. Dots are painted from the centre outward: the innermost ring is red (core city deaths), the next ring is orange (metropolitan area deaths), and the outermost dots remain green (survivors). This concentric layout mirrors the physical reality of a nuclear airburst, where destructive intensity decreases with distance from the detonation point.

### The 200 cities

All city data — population and core city percentage — is embedded directly in the HTML file. No API call or network request is made at any point. The core city percentage for each entry reflects the actual urban morphology of that city: a dense, compact city like Plymouth has a very high core percentage (81%), while a sprawling conglomeration like Manchester has a much lower one (24%), exactly as described in the article.

### Technical stack

The simulator is built in vanilla JavaScript with no ML library dependency, consistent with the author's broader work on implementing algorithms from scratch. The canvas rendering uses a sort-by-distance approach to assign dot colours, which produces the concentric ring effect without any trigonometry.

---

## Français

### Présentation

Ce simulateur est une implémentation visuelle directe du modèle de pertes développé dans l'article *« The consequences of a nuclear war: case study on 80s UK »* (Simon Chabrol, 2025), lui-même fondé sur l'exercice de défense civile britannique **Square Leg** (1980).

L'outil permet à n'importe quel utilisateur de sélectionner une ville parmi 200 prédéfinies — ou d'en saisir une manuellement — et de visualiser les conséquences humaines estimées d'une frappe nucléaire, en utilisant les mêmes formules qu'un planificateur militaire appliquerait.

### Fonctionnement

La simulation repose sur deux formules dérivées de l'article.

La **formule de pertes** calcule un taux de mortalité pondéré entre les deux zones urbaines qui composent toute ville moderne. La zone centrale — la partie la plus dense et la plus urbanisée — se voit attribuer un taux de mortalité de 85 %, reflétant la destruction quasi totale causée par une détonation aérienne directe au-dessus d'une zone urbaine compacte. La zone métropolitaine élargie reçoit un taux de 50 %, tenant compte d'une population plus dispersée et d'une onde de choc moins intense à grande distance. La moyenne pondérée de ces deux taux, appliquée à la population totale, donne le nombre de morts estimé.

```
Pertes = ( (C × DC) + (M × DM) ) / 100

  C  = pourcentage de la population vivant dans le centre-ville
  M  = pourcentage de la population vivant dans l'aire métropolitaine (C + M = 100)
  DC = 85  (taux de mortalité centre-ville, en %)
  DM = 50  (taux de mortalité aire métropolitaine, en %)
```

La **formule mégatonnes** estime ensuite la puissance explosive nécessaire pour produire ces pertes, sur la base de la relation empirique entre une arme de 1,5 MT et une moyenne de 344 000 morts — chiffre validé contre NUKEMAP sur plusieurs villes dans l'article.

```
Mégatonnes = (Morts / 344 000) × MTmoy

  MTmoy = 1,5 (rendement moyen par bombe, ajustable dans l'interface)
```

### La visualisation

La grille de 50×50 points représente la population totale de la ville. Chaque point correspond à une fraction proportionnelle de cette population. Les points sont colorés depuis le centre vers l'extérieur : l'anneau intérieur est rouge (morts en zone centrale), l'anneau suivant est orange (morts en zone métropolitaine), et les points les plus éloignés restent verts (survivants). Cette disposition concentrique reproduit la réalité physique d'une détonation aérienne nucléaire, dont l'intensité destructrice décroît avec la distance.

### Les 200 villes

Toutes les données — population et pourcentage de zone centrale — sont intégrées directement dans le fichier HTML. Aucun appel API ni requête réseau n'est effectué. Le pourcentage de zone centrale de chaque ville reflète sa morphologie urbaine réelle : une ville dense et compacte comme Plymouth a un pourcentage très élevé (81 %), tandis qu'une conurbation étalée comme Manchester a un pourcentage beaucoup plus faible (24 %), exactement comme décrit dans l'article.

### Stack technique

Le simulateur est développé en JavaScript vanilla sans aucune dépendance à une bibliothèque de machine learning, dans la continuité du travail de l'auteur sur l'implémentation d'algorithmes from scratch. Le rendu canvas utilise un tri par distance pour attribuer les couleurs aux points, ce qui produit l'effet d'anneaux concentriques sans aucune trigonométrie.

---

*Based on: Simon Chabrol, "The consequences of a nuclear war: case study on 80s UK" (Medium, 2025)*
*Square Leg exercise data: UK Home Office, 1980*
