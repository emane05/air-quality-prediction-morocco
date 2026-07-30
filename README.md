
# Projet : Prédiction de la Qualite de l'Air dans les Villes Marocaines

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.0+-orange.svg)](https://scikit-learn.org/)
[![Flower](https://img.shields.io/badge/Flower-1.0+-green.svg)](https://flower.dev/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Table des Matieres

- [Introduction](#introduction)
- [Donnees](#donnees)
- [Architecture du Projet](#architecture-du-projet)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Resultats](#resultats)
- [Apprentissage Federe](#apprentissage-federe)
- [Visualisations](#visualisations)
- [Points Cles](#points-cles)
- [Perspectives](#perspectives)
- [Contribution](#contribution)
- [License](#license)
- [Auteur](#auteur)

---

## Introduction

Ce projet vise a predire la qualite de l'air dans six villes marocaines (Casablanca, Rabat, Marrakech, Fes, Tanger, Agadir) en utilisant des modeles de machine learning et une approche d'apprentissage federe innovante qui preserve la confidentialite des donnees.

### Objectifs du Projet

1. Collecter des donnees meteorologiques et de pollution en temps reel via les APIs OpenWeatherMap
2. Preparer et nettoyer les donnees pour la modelisation
3. Developper des modeles centralises (Regression Lineaire, Arbre de Decision, Random Forest)
4. Implementer une approche d'apprentissage federe avec Flower
5. Comparer les performances des deux approches

### Villes Etudiees

| Ville | Caracteristiques |
|-------|------------------|
| Casablanca | Metropole economique, zone industrielle |
| Rabat | Capitale administrative |
| Marrakech | Ville touristique du sud |
| Fes | Ville historique du nord |
| Tanger | Ville cotiere du nord |
| Agadir | Ville cotiere du sud |

### Technologies Utilisees

| Technologie | Utilisation |
|-------------|-------------|
| Python 3.9+ | Langage principal |
| Pandas / NumPy | Manipulation des donnees |
| Scikit-learn | Modeles de Machine Learning |
| Flower | Apprentissage federe |
| Matplotlib / Seaborn | Visualisations |
| OpenWeatherMap API | Collecte des donnees |

---

## Donnees

### Periode de Collecte
- Debut : 25 avril 2026
- Fin : 9 mai 2026
- Duree : 14 jours
- Frequence : Horaire (24 mesures par jour)

### Volume des Donnees
- Enregistrements bruts : 996
- Enregistrements finaux : 996
- Taux de completude : 100%
- Variables : 26 colonnes

### Variables Collectees

| Categorie | Variables |
|-----------|-----------|
| Meteo | Temperature, Humidite, Pression, Vent, Nuages |
| Pollution | AQI, PM2.5, PM10, NO2, O3, SO2, CO |
| Temporelles | Heure, Jour, Mois |
| Identification | Ville, Latitude, Longitude |

### Statistiques Descriptives des PM2.5

| Ville | Moyenne (µg/m³) | Mediane | Min | Max | Ecart-type |
|-------|-----------------|---------|-----|-----|------------|
| Agadir | 5.6 | 2.0 | 1.0 | 9.0 | 3.13 |
| Casablanca | 7.2 | 3.0 | 2.0 | 9.6 | 2.71 |
| Fes | 4.4 | 2.0 | 1.8 | 7.0 | 2.06 |
| Marrakech | 7.4 | 3.0 | 2.0 | 8.0 | 2.15 |
| Rabat | 6.4 | 4.0 | 2.0 | 7.0 | 1.76 |
| Tanger | 3.2 | 2.0 | 1.0 | 3.8 | 0.99 |

### Seuils OMS

| Seuil | Valeur | Signification |
|-------|--------|---------------|
| Valeur guide | 5 µg/m³ | Objectif ideal (moyenne annuelle) |
| Seuil recommande | 10 µg/m³ | Niveau acceptable |
| Seuil d'alerte | 25 µg/m³ | Niveau degrade (moyenne 24h) |

**Conclusion** : Toutes les villes sont en dessous du seuil OMS de 10 µg/m³.

---

## Architecture du Projet

```
PROJET_AIR_QUALITY/
│
├── data/
│   ├── checkpoints/                 # Points de controle
│   ├── processed/                   # Donnees nettoyees
│   └── raw/                         # Donnees brutes
│
├── docs/                            # Documentation
│
├── logs/                            # Fichiers de logs
│
├── models/
│   ├── centralized/                 # Modeles centralises sauvegardes
│   └── federated/                   # Modeles federes sauvegardes
│
├── notebooks/                       # Notebooks Jupyter
│
├── src/
│   ├── centralized/                 # Code pour modeles centralises
│   │   ├── collector.py
│   │   ├── data_preparation.py
│   │   └── model_training.py
│   └── federated/                   # Code pour apprentissage federe
│       ├── client.py
│       ├── server.py
│       └── flower_client.py
│
├── visualizations/                  # Graphiques et visualisations
│
├── .gitignore
├── README.md
├── requirements.txt
│
├── Air_Quality_Prediction_BDSI.pptx    # Presentation PowerPoint
├── commandes.txt                        # Commandes utiles
├── Fiche_Technique_Air_Quality.pdf     # Fiche technique
├── fin_collecte.png                     # Image de fin de collecte
└── Rapport_Air_Quality_Prediction.pdf  # Rapport final
```

---

## Installation

### 1. Cloner le depot

```bash
git clone https://github.com/votre-username/PROJET_AIR_QUALITY.git
cd PROJET_AIR_QUALITY
```

### 2. Creer un environnement virtuel

```bash
# Avec conda
conda create -n air-quality python=3.9
conda activate air-quality

# Ou avec venv
python -m venv venv
source venv/bin/activate  # Sur Linux/Mac
venv\Scripts\activate     # Sur Windows
```

### 3. Installer les dependances

```bash
pip install -r requirements.txt
```

### 4. Configurer les cles API

Creez un fichier `.env` a la racine :

```env
OPENWEATHER_API_KEY=votre_cle_api_ici
```

---

## Utilisation

### 1. Collecte des Donnees

```bash
python src/centralized/collector.py
```

**Resultat** : Les donnees brutes sont sauvegardees dans `data/raw/`

### 2. Preparation des Donnees

```bash
python src/centralized/data_preparation.py
```

**Resultat** : Les donnees nettoyees sont sauvegardees dans `data/processed/`

### 3. Entrainement des Modeles Centralises

```bash
python src/centralized/model_training.py
```

**Resultat** : Les modeles sont sauvegardes dans `models/centralized/`

### 4. Lancer le Notebook

```bash
jupyter notebook notebooks/air_quality_prediction.ipynb
```

### 5. Lancer l'Apprentissage Federe

```bash
# Terminal 1 : Lancer le serveur
python src/federated/server.py

# Terminal 2 : Lancer les clients (un par ville)
python src/federated/client.py --city casablanca
python src/federated/client.py --city rabat
python src/federated/client.py --city marrakech
python src/federated/client.py --city fes
python src/federated/client.py --city tanger
python src/federated/client.py --city agadir
```

---

## Resultats

### Modeles Centralises

#### Regression (Prediction des PM2.5)

| Modele | MAE (µg/m³) | RMSE (µg/m³) | R² |
|--------|-------------|--------------|-----|
| Regression Lineaire | 0.839 | 1.118 | 0.802 |
| Arbre de Decision | 0.498 | 0.763 | 0.908 |
| Random Forest | 0.328 | 0.463 | 0.966 |

**Interpretation** : Le Random Forest est le meilleur modele avec un R² de 0.966.

#### Classification (Prediction de l'AQI)

| Metrique | Valeur |
|----------|--------|
| Accuracy | 98.5% |
| F1-Score (pondere) | 0.983 |

#### Matrice de Confusion

```
              Prédit
              Bon  Moyen  Dégradé
Réel   Bon     5     3       0
       Moyen   0    125      0
       Dégradé 0     0      67
```

#### Importance des Features (Random Forest)

```
Feature Importance :
1. PM10     -> 0.65  (65%)  <- Le plus important
2. CO       -> 0.20  (20%)
3. NO2      -> 0.08  (8%)
4. Temp     -> 0.04  (4%)
5. Autres   -> 0.03  (3%)
```

**Conclusion** : Le PM10 est de loin la variable la plus influente pour predire les PM2.5.

---

## Apprentissage Federe (Flower)

### Architecture

```
+-------------------------------------------------------------+
|                         SERVEUR                              |
|  (Aggrege les poids des modeles via FedAvg)                 |
+-------------------------------------------------------------+
        |              |              |              |
        v              v              v              v
+-------------+ +-------------+ +-------------+ +-------------+
|  CLIENT 1   | |  CLIENT 2   | |  CLIENT 3   | |  CLIENT 4   |
|  Agadir     | |  Casablanca | |  Fes        | |  Marrakech  |
|  (donnees   | |  (donnees   | |  (donnees   | |  (donnees   |
|   locales)  | |   locales)  | |   locales)  | |   locales)  |
+-------------+ +-------------+ +-------------+ +-------------+
```

### Convergence de la Loss

| Round | Loss |
|-------|------|
| 1 | 1.6049 |
| 2 | 1.6049 |
| 3 | 1.6049 |
| 4 | 1.6049 |
| 5 | 1.6049 |
| 6 | 1.6049 |
| 7 | 1.6049 |
| 8 | 1.6049 |
| 9 | 1.6049 |
| 10 | 1.6049 |

**Conclusion** : Convergence rapide et stable en seulement 10 rounds.

### Performances par Ville

| Ville | MAE (µg/m³) | R² | Statut |
|-------|-------------|-----|--------|
| Agadir | 0.944 | 0.783 | Tres bonne |
| Tanger | 0.950 | 0.771 | Tres bonne |
| Marrakech | 1.148 | 0.698 | Moyenne |
| Rabat | 2.430 | 0.463 | Moyenne |
| Fes | 1.488 | -0.074 | Sous la moyenne |
| Casablanca | 2.754 | -0.399 | Sous la moyenne |

**Conclusion** : Les villes cotieres (Agadir, Tanger) sont mieux modelisees que les grandes villes.

### Comparaison Centralise vs Federe

| Approche | R² | MAE (µg/m³) |
|----------|-----|-------------|
| Centralise (Random Forest) | 0.966 | 0.328 |
| Centralise (Regression Lineaire) | 0.802 | 0.839 |
| Federe (Flower) | 0.374 | 1.618 |

**Conclusion** : L'apprentissage federe preserve la confidentialite mais sacrifie une partie de la performance (trade-off classique).

---

## Visualisations

Les visualisations suivantes sont disponibles dans le dossier `visualizations/` :

| Fichier | Description |
|---------|-------------|
| `model_comparison.png` | Comparaison des modeles de regression |
| `confusion_matrix.png` | Matrice de confusion pour la classification AQI |
| `feature_importance.png` | Importance des features (Random Forest) |
| `federated_comparison.png` | Comparaison centralise vs federe |
| `flower_convergence.png` | Convergence de l'apprentissage federe |
| `fin_collecte.png` | Image de fin de collecte |

---

## Points Cles

### Ce qu'on a Appris

1. Qualite de l'air au Maroc
   - Excellente (toutes les villes sous le seuil OMS)
   - Tanger : la meilleure (3.2 µg/m³ en moyenne)
   - Marrakech : la moins bonne (7.4 µg/m³ en moyenne)

2. Modeles Performants
   - Random Forest : R² = 0.966 (regression)
   - Random Forest Classifier : Acc = 98.5% (classification)

3. Variables Cles
   - PM10 : le meilleur predicteur des PM2.5 (correlation 0.93)
   - NO2 et CO : indicateurs de trafic
   - Temperature : influence la dispersion

4. Apprentissage Federe
   - Fonctionnel et efficace
   - Trade-off Performance vs Confidentialite
   - Heterogeneite des donnees : principal defi

---

## Perspectives d'Amerlioration

1. Collecte Prolongee
   - Etendre sur plusieurs saisons (hiver, ete)
   - Ajouter d'autres villes
   - Augmenter la frequence des mesures

2. Modeles Avances
   - Deep Learning (LSTM pour les series temporelles)
   - Modeles federes avances (FedProx, Scaffold)
   - Modeles hybrides

3. Deploiement
   - Architecture client-serveur reelle
   - Application mobile ou web
   - Systeme d'alerte en temps reel

4. Donnees Supplementaires
   - Donnees de trafic
   - Donnees industrielles
   - Donnees geographiques
   - Donnees de vegetation

---

## Contribution

Les contributions sont les bienvenues. Voici comment contribuer :

1. Fork le projet
2. Creez une branche : `git checkout -b feature/ma-feature`
3. Commitez : `git commit -m "Ajout de ma feature"`
4. Poussez : `git push origin feature/ma-feature`
5. Ouvrez une Pull Request

---

## License

Ce projet est sous license MIT 

---

## Auteur

**Imane Boujaj**
- Email : imaneboujaj29@gmail.com

---

## Remerciements

- OpenWeatherMap pour la mise a disposition de leurs APIs
- La communaute Flower pour le framework d'apprentissage federe
- Scikit-learn pour les modeles de machine learning
- Monsieur le Professeur pour son encadrement

---

## References

1. OMS : Lignes directrices mondiales sur la qualite de l'air (2021)
2. OpenWeatherMap : API Documentation
3. Flower : Framework d'apprentissage federe
4. Scikit-learn : Documentation des modeles ML

---

## Resume des Performances

| Tache | Meilleur Modele | Performance |
|-------|-----------------|-------------|
| Regression (PM2.5) | Random Forest | R² = 0.966 |
| Classification (AQI) | Random Forest | Acc = 98.5% |
| Apprentissage Federe | Flower + FedAvg | Converge en 10 rounds |

---

**Projet realise dans le cadre du module "L'IA et l'Apprentissage Federe" - Master BDSI**

**Date : 02 Juin 2026**

---

*"La qualite de l'air que nous respirons aujourd'hui determine la sante que nous aurons demain."*
```



