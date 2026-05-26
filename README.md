# Analyse de l'évolution de la circulation automobile au Québec à partir de données ouvertes

Ce répertoire permet de visualiser l'évolution de différents indicateurs de la circulation au Québec à partir de données ouvertes du Ministère des Transports et de la Mobilité Durable (MTMD). Cette analyse poursuit le travail réalisé par TARCÍSIO COSTA DE SOUZA NETO en 2023 dans le cadre de sa maîtrise professionnelle en génie civil à Polytechnique Montréal.

L'analyse originale de TARCÍSIO COSTA DE SOUZA NETO se trouve à l'adresse suivante : [https://github.com/nsaunier/OpenDataQuebec/tree/main/QuebecTrafficVolumes](https://github.com/nsaunier/OpenDataQuebec/tree/main/QuebecTrafficVolumes).

Le présent travail a été réalisé par Léry Tanguay à l'hiver 2026, sous la supervision du professeur Nicolas Saunier, dans le cadre d'un projet personnel à la maîtrise professionnelle en génie civil à Polytechnique Montréal.

### Flux de travail

* **`telecharger_fichiers.py`** : Permet de télécharger les données ouvertes de comptage permanents et ponctuels issues du MTMD.
* **Fichiers d'analyse** : `VKT_analysis.ipynb` et `permanent_sensors_data_analysis.ipynb` sont les fichiers d'analyse qui se basent sur ces données téléchargées. Ils utilisent des fonctions du fichier `MyFunctionsOpenQuebec.py` pour extraire les données préalablement téléchargées.

### Aperçu de l'analyse

#### `permanent_sensors_data_analysis.ipynb`
Ce fichier se base sur les données des capteurs permanents. Il permet de visualiser l'évolution annuelle des indicateurs suivants sur les sections mesurées :
* Débit journalier moyen annuel (DJMA)
* Débit journalier moyen mensuel (DJMM)
* Débit journalier annuel moyen par jour de la semaine
* Débit horaire annuel moyen

La visualisation est faite sur l'ensemble des sections et par groupe (région, classification de la section et sections les plus achalandées).

#### `VKT_analysis.ipynb`
Ce fichier se base sur les données de capteurs permanents et, indirectement, sur les données de comptages temporaires (le MTMD fournit des estimations de DJMA pour certaines sections à partir de comptages temporaires).
* **Étape 1** : Estimer les DJMA manquants sur les sections pour lesquelles le DJMA est connu pour au moins une année. La méthode consiste à trouver pour chaque section, tronçon, route et région, le changement relatif moyen de DJMA d'une année à l'autre,  et d'appliquer un de ces changements relatifs (l'odre de priorité est: section, tronçon, route et région) à un DJMA connu d'une section pour estimer un DJMA inconnu.
* **Étape 2** : Estimer les DJMA pour les sections n'ayant aucune valeur. Pour chaque section 'inconnue', les DJMA estimés correspondent à la moyenne des DJMA par année des sections à proximité sur la même route, ou à la moyenne sur le tronçon ou la route dans le cas où il n'y a pas de section 'connue' à proximité.
* **Étape 3** : Les VKT (Vehicle Kilometers Traveled) annuels par région sont estimés à partir des DJMA et de la longueur des segments.

### Sources de données et environnement
En plus des données téléchargées automatiquement avec le fichier `telecharger_fichiers.py`, d'autres fichiers sont utilisés dans l'analyse :
* [Géométrie et données de DJMA estimées des segments (shapefile)](https://www.donneesquebec.ca/recherche/dataset/debit-de-circulation/resource/9de14998-2e3b-4936-a587-2da4f3ddd3af)
* [Géométrie des régions du Québec (shapefile)](https://www.donneesquebec.ca/recherche/dataset/decoupages-administratifs/resource/a30825b5-82d2-491d-9783-c513fe36f231)
* [Fichier RTSS contenant notamment la classification des RTSS (geopackage)](https://www.donneesquebec.ca/recherche/dataset/reseau-routier-rtss/resource/d09ba078-fd84-4e2e-96f5-178dd346564b)
* L'environnement dans lequel l'analyse a été réalisée peut être reproduit à partir du fichier `environment.yml` ou du fichier `requirements.txt`.

---

# Analysis of Traffic Volume Trends in Quebec Using Open Data

This repository provides tools to visualize various traffic indicators in Quebec using open data from the *Ministère des Transports et de la Mobilité Durable* (MTMD). This work builds upon the 2023 research conducted by Tarcísio Costa de Souza Neto as part of his professional Master’s degree in Civil Engineering at Polytechnique Montréal.

The original work by Tarcísio Costa de Souza Neto can be found here: [https://github.com/nsaunier/OpenDataQuebec/tree/main/QuebecTrafficVolumes](https://github.com/nsaunier/OpenDataQuebec/tree/main/QuebecTrafficVolumes).

This project carried out by Léry Tanguay in the Winter of 2026, under the supervision of Professor Nicolas Saunier, as part of a personal project in his Master's degree in Civil Engineering at Polytechnique Montréal.

### Workflow

* **`telecharger_fichiers.py`**: Downloads open-access permanent and temporary traffic count data from the MTMD portal.
* **Analysis Files**: `VKT_analysis.ipynb` and `permanent_sensors_data_analysis.ipynb` serve as the main analysis workbooks. They utilize helper functions defined in `MyFunctionsOpenQuebec.py` to extract and process the downloaded datasets.

### Analysis Overview

#### `permanent_sensors_data_analysis.ipynb`
This notebook focuses on data from permanent traffic sensors. It visualizes the annual evolution of key indicators, including:
* Annual Average Daily Traffic (AADT / DJMA)
* Monthly Average Daily Traffic (MADT / DJMM)
* AADT by day of the week
* Average Annual Hourly Traffic
Visualizations are generated at both the aggregate level and by specific groups (region, road classification, and high-traffic segments).

#### `VKT_analysis.ipynb`
This notebook relies on permanent sensor data as well as AADT esitmation from the MTMD based on temporary traffic counts.
* **Step 1:** Estimate the missing AADT values for sections for which the AADT is known for at least one year. The method involves determining, for each section, segment, road, and region, the average relative change in AADT from one year to the next, and applying one of these relative changes (in the following order of priority: section, segment, road, and region) to a known DJMA for a section to estimate an unknown DJMA.
* **Step 2:** Estimating AADT for sections with no historical data. "Unknown" sections are assigned the average AADT of nearby sections on the same route. If no nearby data is available, the estimate is based on the segment or route average for that year.
* **Step 3:** Calculating annual Vehicle Kilometers Traveled (VKT) per region based on AADT and segment lengths.

### Data Sources and Environment
In addition to the data automatically fetched by `telecharger_fichiers.py`, the analysis utilizes the following files:
* [Geometry and estimated AADT segment data (Shapefile)](https://www.donneesquebec.ca/recherche/dataset/debit-de-circulation/resource/9de14998-2e3b-4936-a587-2da4f3ddd3af)
* [Quebec administrative regions (Shapefile)](https://www.donneesquebec.ca/recherche/dataset/decoupages-administratifs/resource/a30825b5-82d2-491d-9783-c513fe36f231)
* [RTSS file including network classification (GeoPackage)](https://www.donneesquebec.ca/recherche/dataset/reseau-routier-rtss/resource/d09ba078-fd84-4e2e-96f5-178dd346564b)
* The environment in which the analysis was performed can be reproduced using the `environment.yml` file or the `requirements.txt` file.

---
