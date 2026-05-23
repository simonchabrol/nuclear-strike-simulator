# Nuclear Strike Simulator / Simulateur de frappe nucléaire

## English

### Overview

This simulator is a direct visual implementation of the casualty model developed in the article *"The consequences of a nuclear war: case study on 80s UK"* (Simon Chabrol, 2025), itself based on the British government's **Square Leg** civil defence exercise (1980).

The tool allows any user to select one of 200 pre-loaded cities — or enter a city manually — and visualise the estimated human consequences of a nuclear strike. The blast zones are projected onto an interactive map, with circle radii calculated from the Glasstone-Dolan scaling law and automatically adjusted to the total explosive yield required by each city.

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

The **megatons formula** then estimates the total explosive power required to produce those casualties, based on the empirical relationship between a 1.5 MT weapon and an average of 344,000 deaths — a figure validated against NUKEMAP across multiple cities in the article.

```
Megatons = (Deaths / 344,000) × AvgMT

  AvgMT = 1.5 (average yield per bomb, adjustable in the interface)
```

### The map visualisation

Once a city is selected and the simulation is run, two concentric transparent circles are drawn over an OpenStreetMap base layer. The inner red circle represents the severe destruction zone (approximately 5 psi overpressure, corresponding to the 85% death rate applied to the core city). The outer orange circle represents the moderate damage and fallout zone (approximately 1 psi, corresponding to the 50% death rate applied to the metropolitan area). A crosshair marks the point of detonation at the city centre.

The radii of both circles are derived from the **Glasstone-Dolan scaling law**, which establishes that blast radius scales with the cube root of the weapon yield:

```
r = k × W^(1/3)

  r = radius in km
  W = total yield in megatons
  k = 2.2 (severe destruction zone) / 6.0 (moderate damage zone)
```

Because the total yield varies significantly between cities — from under 1 MT for small towns to over 100 MT for megacities — the map zoom level is adjusted automatically when a city is selected, ensuring the blast circles remain visible and proportionate within the viewport.

### The 200 cities

All city data — population, core city percentage, and geographic coordinates — is embedded directly in the HTML file. No API call or network request is made beyond loading the map tiles from OpenStreetMap. The core city percentage for each entry reflects the actual urban morphology of that city: a dense, compact city like Plymouth has a very high core percentage (81%), while a sprawling conglomeration like Manchester has a much lower one (24%), exactly as described in the article.

### Technical stack

The interface is built in vanilla JavaScript using Leaflet.js for the map layer. The blast circle overlay is drawn on a transparent HTML canvas element positioned above the Leaflet map container, with `pointer-events: none` so the map remains fully interactive. Radii are recomputed in pixels at every zoom and pan event, ensuring the circles remain geographically accurate at any scale. The zoom level is derived by inverting the Glasstone-Dolan formula against the target viewport dimensions.

---

## Français

### Présentation

Ce simulateur est une implémentation visuelle directe du modèle de pertes développé dans l'article *« The consequences of a nuclear war: case study on 80s UK »* (Simon Chabrol, 2025), lui-même fondé sur l'exercice de défense civile britannique **Square Leg** (1980).

L'outil permet à n'importe quel utilisateur de sélectionner une ville parmi 200 prédéfinies — ou d'en saisir une manuellement — et de visualiser les conséquences humaines estimées d'une frappe nucléaire. Les zones de destruction sont projetées sur une carte interactive, avec des rayons calculés à partir de la loi d'échelle de Glasstone-Dolan et automatiquement ajustés à la puissance explosive totale requise par chaque ville.

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

La **formule mégatonnes** estime ensuite la puissance explosive totale nécessaire pour produire ces pertes, sur la base de la relation empirique entre une arme de 1,5 MT et une moyenne de 344 000 morts — chiffre validé contre NUKEMAP sur plusieurs villes dans l'article.

```
Mégatonnes = (Morts / 344 000) × MTmoy

  MTmoy = 1,5 (rendement moyen par bombe, ajustable dans l'interface)
```

### La visualisation cartographique

Une fois une ville sélectionnée et la simulation lancée, deux cercles concentriques transparents sont tracés par-dessus un fond de carte OpenStreetMap. Le cercle rouge intérieur représente la zone de destruction sévère (environ 5 psi de surpression, correspondant au taux de mortalité de 85 % appliqué au centre-ville). Le cercle orange extérieur représente la zone de dommages modérés et de retombées (environ 1 psi, correspondant au taux de 50 % appliqué à l'aire métropolitaine). Un viseur marque le point de détonation au centre de la ville.

Les rayons des deux cercles sont dérivés de la **loi d'échelle de Glasstone-Dolan**, qui établit que le rayon de destruction croît avec la racine cubique du rendement de l'arme :

```
r = k × W^(1/3)

  r = rayon en km
  W = puissance totale en mégatonnes
  k = 2,2 (zone de destruction sévère) / 6,0 (zone de dommages modérés)
```

La puissance totale variant considérablement d'une ville à l'autre — de moins d'1 MT pour de petites villes à plus de 100 MT pour les mégalopoles — le niveau de zoom de la carte est ajusté automatiquement à la sélection de chaque ville, pour que les cercles restent visibles et proportionnés dans la fenêtre d'affichage.

### Les 200 villes

Toutes les données — population, pourcentage de zone centrale et coordonnées géographiques — sont intégrées directement dans le fichier HTML. Aucun appel API n'est effectué au-delà du chargement des tuiles cartographiques depuis OpenStreetMap. Le pourcentage de zone centrale de chaque ville reflète sa morphologie urbaine réelle : une ville dense et compacte comme Plymouth a un pourcentage très élevé (81 %), tandis qu'une conurbation étalée comme Manchester a un pourcentage beaucoup plus faible (24 %), exactement comme décrit dans l'article.

### Stack technique

L'interface est développée en JavaScript vanilla avec Leaflet.js pour la couche cartographique. L'overlay des cercles est dessiné sur un élément `<canvas>` transparent positionné au-dessus du conteneur Leaflet, avec `pointer-events: none` pour que la carte reste entièrement interactive. Les rayons sont recalculés en pixels à chaque événement de zoom et de déplacement, garantissant que les cercles restent géographiquement corrects à toutes les échelles. Le niveau de zoom est dérivé en inversant la formule de Glasstone-Dolan par rapport aux dimensions cibles de la fenêtre d'affichage.

---

*Based on: Simon Chabrol, "The consequences of a nuclear war: case study on 80s UK" (Medium, 2025)*
*Square Leg exercise data: UK Home Office, 1980*
