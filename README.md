# 📉 Prédiction du Churn Client — Opérateur Télécom

> Projet end-to-end de classification binaire visant à identifier les clients susceptibles de se désabonner, afin de réduire le taux de churn et cibler les actions de rétention.

---

## 🎯 Objectif Business

Un opérateur télécom cherche à **réduire son taux de désabonnement** (churn). L'enjeu : détecter à l'avance les clients à risque pour déclencher des actions de fidélisation ciblées (offres, rappels, remises).

- **Variable cible** : `churn` — `1` = client parti, `0` = client fidèle
- **Métrique principale** : Recall (minimiser les churners non détectés)
- **Métriques secondaires** : F1-score, AUC-ROC

---

## 📁 Structure du projet

```
churn/
├── churn.ipynb              # Notebook principal (pipeline complet)
├── ChurnData.csv            # Dataset source
├── churn_best_model.pkl     # Modèle final sauvegardé (joblib)
└── README.md
```

---

## 📊 Dataset

| Caractéristique | Détail |
|---|---|
| Source | IBM / Kaggle (télécom) |
| Observations | ~1 000 clients |
| Features | 27 variables (profil, services, dépenses) |
| Variable cible | `churn` (binaire) |
| Déséquilibre | 71% fidèles / 29% churners |

**Catégories de variables :**
- Profil client : `tenure`, `age`, `income`, `ed`, `employ`, `address`
- Services souscrits : `equip`, `internet`, `wireless`, `confer`, `ebill`...
- Dépenses mensuelles : `longmon`, `tollmon`, `equipmon`, `cardmon`, `wiremon`
- Durées d'utilisation : `longten`, `tollten`, `cardten`
- Variables transformées : `loglong`, `logtoll`, `lninc` (normalisées par log)

---

## 🔬 Méthodologie (CRISP-DM)

```
1. Data Understanding   →  Compréhension du problème et des variables
2. Data Collection      →  Chargement, inspection, doublons, valeurs manquantes
3. EDA                  →  Analyse univariée, bivariée, tests statistiques, corrélation
4. Preprocessing        →  Séparation X/y, train/test split stratifié, Pipelines
5. Modeling             →  4 modèles comparés avec validation croisée
6. Hyperparameter Tuning→  GridSearchCV (scoring=recall)
7. Évaluation finale    →  Sélection du meilleur modèle, courbes ROC, feature importances
8. Déploiement          →  Sauvegarde joblib
```

---

## 🧪 Tests statistiques

| Variable | Test utilisé | Résultat |
|---|---|---|
| `tenure`, `income`, `longmon`, `longten` | Mann-Whitney U | Significatif (p < 0.05) |
| `equip`, `internet`, `confer`, `ebill`... | Chi-2 (χ²) | Plusieurs significatifs |

> Mann-Whitney U choisi (non-paramétrique) car les distributions ne suivent pas une loi normale. Chi-2 appliqué sur les variables binaires de services.

---

## 🤖 Modèles entraînés

Chaque modèle est encapsulé dans un **Pipeline scikit-learn** (StandardScaler + estimateur) pour éviter toute fuite de données entre le train et le test set.

| Modèle | Gestion du déséquilibre |
|---|---|
| Régression Logistique | `class_weight="balanced"` |
| SVM | `class_weight="balanced"` |
| Random Forest | `class_weight="balanced"` |
| XGBoost | `scale_pos_weight` calculé |

---

## 📈 Résultats

Les modèles sont comparés sur le test set après GridSearchCV :

| Modèle | Recall | F1-score | AUC-ROC |
|---|---|---|---|
| Régression Logistique | — | — | — |
| SVM | — | — | — |
| Random Forest | — | — | — |
| XGBoost | — | — | — |

> *Renseigner les valeurs après exécution du notebook.*

Le meilleur modèle est sélectionné automatiquement selon le **Recall** et sauvegardé dans `churn_best_model.pkl`.

---

## 🔑 Variables les plus discriminantes

D'après l'analyse bivariée et l'importance des features (Random Forest / XGBoost) :

1. `tenure` — les clients récents sont les plus à risque
2. `income` — les faibles revenus augmentent la sensibilité au prix
3. `longmon` / `longten` — faible usage = faible engagement
4. Multi-services (`internet`, `confer`, `ebill`) — effet de lock-in protecteur
5. `equipmon` — coût de sortie dissuasif

---

## ⚙️ Installation & Exécution

```bash
# Cloner le dépôt
git clone https://github.com/Gracias-Leader/<nom-du-repo>.git
cd <nom-du-repo>

# Installer les dépendances
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib

# Lancer le notebook
jupyter notebook churn.ipynb
```

---

## 🛠️ Stack technique

![Python](https://img.shields.io/badge/Python-3.10-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-orange)
![XGBoost](https://img.shields.io/badge/XGBoost-2.x-red)
![Pandas](https://img.shields.io/badge/Pandas-2.x-lightblue)
![Seaborn](https://img.shields.io/badge/Seaborn-0.13-green)

`pandas` · `numpy` · `matplotlib` · `seaborn` · `scipy` · `scikit-learn` · `xgboost` · `joblib`

---

## 👤 Auteur

**Bertin** — Data Science Practitioner  
📍 Pointe-Noire, Congo  
🎓 Licence Économie Quantitative — Université Marien Ngouabi  
📜 IBM Data Science & Data Analyst Professional Certificates (Coursera)

---

## 📄 Licence

Projet personnel à vocation éducative et portfolio. Dataset IBM — usage libre pour l'apprentissage.
