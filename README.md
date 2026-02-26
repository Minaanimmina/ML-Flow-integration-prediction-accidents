# 🚦 Prédiction de la Gravité des Accidents Routiers — Intégration MLflow

## 📋 Description
Ce projet intègre MLflow dans un pipeline de machine learning pour la prédiction 
de la gravité des accidents routiers (données BAAC).
Il ajoute une couche MLOps complète : tracking des expériences, tuning 
d'hyperparamètres et versioning des modèles.

---

## 🏗️ Architecture du projet
```
├── notebooks/
│   └── mlflow.ipynb          # notebook principal
├── data/
│   └── raw/accidents.csv     # données BAAC
├── docker-compose.yml        # service MLflow dockerisé
├── .gitignore
└── README.md
```

---

## ⚙️ Installation

### Prérequis
- Python 3.10+
- Docker Desktop

### Installation des dépendances
```bash
uv sync

```

---

## 🚀 Lancer MLflow

### Option 1 — En local
```bash
mlflow server --host 127.0.0.1 --port 5000
```
Interface disponible sur : http://127.0.0.1:5000

### Option 2 — Avec Docker (recommandé)
```bash
docker-compose up
```
Interface disponible sur : http://localhost:5000

---

## 🧪 Reproduire les expériences

### 1. Lancer le serveur MLflow (voir ci-dessus)

### 2. Ouvrir le notebook
```bash
jupyter notebook notebooks/mlflow.ipynb
```

### 3. Exécuter les cellules dans l'ordre
| Cellule | Description |
|---|---|
| Configuration | Tracking URI + expériences |
| Baseline | Entraînement des 4 modèles |
| Tuning manuel | 3 configurations XGBoost |
| GridSearchCV | 27 combinaisons automatiques |
| Optuna | 30 trials bayésiens |
| Model Registry | Enregistrement du meilleur modèle |

---

## 📊 Résultats

| Approche | Recall | ROC AUC |
|---|---|---|
| Baseline XGBoost | 0.6621 | 0.8377 |
| GridSearchCV | 0.6647 | 0.8404 |
| **Optuna** | **0.6680** | - |

> Le recall a été choisi comme métrique de référence car dans un contexte 
> d'accidents routiers, minimiser les faux négatifs sur les cas graves est prioritaire.

---

## 🐳 Docker

Le fichier `docker-compose.yml` contient le service MLflow avec :
- Backend SQLite persistant
- Volumes pour les artefacts
- Interface web sur le port 5000
```bash
# Lancer
docker-compose up

# Arrêter
docker-compose down
```

---

## 🗂️ Expériences MLflow

| Expérience | Contenu |
|---|---|
| `accidents-gravite-baseline` | 4 modèles avec paramètres par défaut |
| `accidents-gravite-tuning` | Tuning manuel XGBoost (3 configs) |
| `accidents-gravite-gridsearch` | GridSearchCV (27 combinaisons) |
| `accidents-gravite-optuna` | Optuna bayésien (30 trials) |

---

## 🏆 Model Registry

Le meilleur modèle est enregistré sous le nom `xgboost-gravite-accidents`.

| Version | Approche | Recall | Statut |
|---|---|---|---|
| v1 | Baseline XGBoost | 0.6621 | Archived |
| v2 | Optuna | 0.6680 | Staging |