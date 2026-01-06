# Travaux Pratiques - Data Cleansing

Collection de notebooks Jupyter sur le nettoyage de données avec Python.

---

## Contenu

### TP1 - Découverte et Valeurs Manquantes Simples
**Difficulté:** Débutant  
**Dataset:** `ecommerce_simple.csv`  
**Objectif:** Introduction au nettoyage avec gestion basique des valeurs manquantes

### TP2 - Doublons et Standardisation
**Difficulté:** Débutant  
**Dataset:** `customers_duplicates.csv`  
**Objectif:** Détection et suppression des doublons, standardisation des formats

---

## TP1 - Valeurs Manquantes

### Concepts abordés

- **Détection des valeurs manquantes**
  - Comptage avec `df.isnull().sum()`
  - Calcul des pourcentages
  - Visualisation avec `missingno`

- **Stratégies d'imputation**
  - **Médiane** pour variables numériques (résistante aux outliers)
  - **Mode** pour variables catégorielles (valeur la plus fréquente)
  - Suppression de colonnes avec >70% de valeurs manquantes

- **Analyse exploratoire (EDA)**
  - `df.head()`, `df.info()`, `df.describe()`
  - Corrélations entre variables
  - Distribution des données

### Étapes du TP

1. Import et exploration du dataset (500 lignes, 8 colonnes)
2. Analyse des informations générales
3. Calcul du pourcentage de valeurs manquantes par colonne
4. Suppression des colonnes avec >70% de valeurs manquantes
5. Remplissage des valeurs numériques avec la médiane
6. Remplissage des valeurs catégorielles avec le mode
7. Vérification de l'absence de valeurs manquantes
8. Résumé des transformations effectuées

### Métriques clés

| Métrique | Avant | Après |
|----------|-------|-------|
| Colonnes avec valeurs manquantes | Variable | 0 |
| Valeurs manquantes totales | Variable | 0 |

### Code exemple

```python
# Remplir avec médiane
numeric_cols = df.select_dtypes(include=['int64', 'float64']).columns
for col in numeric_cols:
    if df[col].isnull().sum() > 0:
        median_value = df[col].median()
        df[col].fillna(median_value, inplace=True)

# Remplir avec mode
categorical_cols = df.select_dtypes(include=['object']).columns
for col in categorical_cols:
    if df[col].isnull().sum() > 0:
        mode_value = df[col].mode()[0]
        df[col].fillna(mode_value, inplace=True)
```

---

## TP2 - Doublons et Standardisation

### Concepts abordés

- **Détection de doublons**
  - Doublons exacts avec `df.duplicated()`
  - Identification des lignes en doublon
  - Pourcentage de duplication

- **Suppression de doublons**
  - Conservation de la première occurrence avec `keep='first'`
  - Impact sur la taille du dataset

- **Standardisation des données**
  - **Casse** : emails en minuscules, noms en title case
  - **Espaces** : suppression avec `.str.strip()`
  - **Catégories** : mapping pour unifier les variations (M/Male/m → Male)
  - **Formats** : téléphones au format international

### Étapes du TP

1. Identification et comptage des doublons exacts
2. Affichage d'exemples de doublons
3. Suppression des doublons (conservation de la première occurrence)
4. Standardisation de la casse (emails, noms)
5. Suppression des espaces en début/fin de chaînes
6. Création de mappings pour les catégories
7. Application des mappings
8. Standardisation des formats de téléphone
9. Vérification des doublons restants après standardisation
10. Création du rapport de nettoyage

### Transformations appliquées

#### Standardisation de la casse
```python
df['email'] = df['email'].str.lower()
df['name'] = df['name'].str.title()
```

#### Mapping des catégories
```python
gender_mapping = {
    'M': 'Male', 'm': 'Male', 'Male': 'Male',
    'F': 'Female', 'f': 'Female', 'Female': 'Female'
}
df['gender'] = df['gender'].map(gender_mapping)
```

#### Standardisation des téléphones
```python
import re
def clean_phone(phone):
    cleaned = re.sub(r'\D', '', str(phone))
    return f"+{cleaned[:2]} {cleaned[2:5]} {cleaned[5:8]} {cleaned[8:]}"

df['phone'] = df['phone'].apply(clean_phone)
```

### Métriques clés

| Métrique | Valeur |
|----------|--------|
| Doublons supprimés | Variable selon dataset |
| Taux de compression | Variable |
| Catégories standardisées | Gender, Country, etc. |
| Formats unifiés | Téléphones, emails |

---

## Technologies utilisées

- **Python** 3.12
- **pandas** : Manipulation de données
- **numpy** : Calculs numériques
- **matplotlib** : Visualisations
- **seaborn** : Graphiques statistiques
- **missingno** : Visualisation des valeurs manquantes
- **scipy** : Statistiques (TP3+)

---

## Installation

```bash
pip install pandas numpy matplotlib seaborn missingno scipy
```

Ou avec le fichier requirements.txt :

```bash
pip install -r requirements.txt
```

---

## Utilisation

1. Cloner le repository
```bash
git clone https://github.com/[votre-username]/data-cleansing-tp.git
cd data-cleansing-tp
```

2. Installer les dépendances
```bash
pip install -r requirements.txt
```

3. Ouvrir un notebook
```bash
jupyter notebook TP1_Data_Cleansing_COMPLETED.ipynb
```

---

## Structure des notebooks

Chaque TP suit une structure cohérente :

1. **Imports** : Bibliothèques nécessaires
2. **Import du dataset** : Chargement des données
3. **Visualisation** : Analyse des valeurs manquantes
4. **Questions préalables** : 10 questions avec réponses
5. **Étapes du TP** : 8-10 étapes guidées
6. **Rapport final** : Résumé des transformations

---

## Compétences développées

### TP1
- Détection et analyse des valeurs manquantes
- Choix de stratégies d'imputation appropriées
- Utilisation de médianes et modes
- Manipulation de DataFrames pandas
- Visualisation de données

### TP2
- Détection de doublons
- Déduplication de données
- Standardisation de formats
- Nettoyage de chaînes de caractères
- Mapping de catégories
- Expressions régulières (regex)

---

### Concepts clés

**Pourquoi la médiane plutôt que la moyenne ?**
- La médiane est résistante aux valeurs extrêmes
- Exemple : [10, 20, 30, 1000] → moyenne=265, médiane=25
- Pour des données avec outliers, la médiane est plus représentative

**Pourquoi le mode pour les catégories ?**
- Le mode préserve la distribution des catégories
- C'est la valeur la plus probable statistiquement
- Évite d'introduire des valeurs qui n'existent pas dans les données

**Quand supprimer vs imputer ?**
- Supprimer si >70% de valeurs manquantes
- Imputer si les valeurs manquantes sont aléatoires (MCAR)
- Analyser le pattern de missingness avant de décider

---
