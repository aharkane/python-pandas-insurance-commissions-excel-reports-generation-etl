# Analyse des Commissions Courtiers

## Vue d'ensemble du projet

Un projet d'**automatisation et d'analyse de données** utilisant Python pour traiter les commissions d'assurance de plusieurs courtiers. Ce projet démontre des compétences en manipulation de données avec Pandas, automatisation de processus ETL, et génération de rapports structurés pour l'analyse financière.

## Objectifs du projet

- Automatiser le traitement des données de commissions provenant d'un fichier Excel consolidé
- Transformer et agréger les commissions par courtier et type d'affaire
- Générer automatiquement des fichiers Excel individuels pour chaque courtier
- Faciliter l'analyse des performances commerciales par type de produit d'assurance

## Réalisations clés

✓ Agrégation de données de commissions multi-courtiers  
✓ Automatisation complète du processus ETL (Extract-Transform-Load)  
✓ Génération automatique de 7 fichiers Excel individuels  
✓ Organisation des données par type d'affaire avec onglets séparés  
✓ Gestion des commissions positives et négatives (reprises)  
✓ Code réutilisable et modulaire pour d'autres analyses similaires  

## Compétences démontrées
<table>
<tr>
<td width="55%" valign="top">
  
**Manipulation de données:**
- Lecture et écriture de fichiers Excel avec Pandas
- Agrégation avec groupby et fonctions d'agrégation (sum)
- Réinitialisation d'index avec reset_index
- Filtrage de DataFrames avec conditions booléennes

**Automatisation:**
- Boucles itératives sur valeurs uniques
- Génération dynamique de fichiers
- Gestion de système de fichiers avec os.makedirs
- Context managers (with statement) pour gestion de ressources
</td>
<td width="50%" valign="top">
  
**Traitement financier:**
- Calcul de totaux de commissions
- Gestion des valeurs négatives (reprises)
- Segmentation par type de produit d'assurance
- Reporting structuré par entité commerciale

**Développement Python:**
- Programmation Jupyter Notebook
- Utilisation de bibliothèques scientifiques (Pandas, NumPy)
- Code propre et documenté
- Gestion d'erreurs potentielles
</td>
</tr>
</table>

## Stack technologique

| Composant | Technologie |
|-----------|-----------|
| **Langage** | Python 3.x |
| **Traitement de données** | Pandas, NumPy |
| **Format de fichiers** | Excel (XLSX) |
| **Environnement** | Jupyter Notebook |
| **Automatisation** | Boucles itératives, os.makedirs |

## Données analysées
## Structure du dépôt

```
├── analyse-commissions-courtiers.ipynb
├── produits-courtiers.xlsx
├── Fichiers/
│   ├── Courtier001.xlsx
│   ├── Courtier002.xlsx
│   ├── Courtier003.xlsx
│   ├── Courtier004.xlsx
│   ├── Courtier005.xlsx
│   ├── Courtier006.xlsx
│   └── Courtier007.xlsx
└── README.md
```

**Fichier source: produits-courtiers.xlsx**
- Données de commissions d'assurance multi-courtiers
- Types d'affaires: Santé et Prévoyance
- Catégories de commissions:
  - Détail des affaires différées
  - Détail des affaires nouvelles commissionnées
  - Détail des affaires récurrentes
  - Détail des reprises de commissions
  - Frais de reversement

**Courtiers inclus:**
- Courtier001 à Courtier007
- Chaque courtier avec plusieurs types d'affaires
- Montants de commissions variables (positifs et négatifs pour les reprises)

## Processus d'analyse

### 1. Chargement et exploration des données

```python
import pandas as pd
import numpy as np
import os

# Chargement du fichier Excel
df = pd.read_excel('./produits-courtiers.xlsx')
```

### 2. Agrégation des commissions

```python
# Agrégation par courtier et type d'affaire
df2 = (df
    .groupby(['Nom courtier', 'Type d\'affaire'])['Commissions']
    .sum()
    .reset_index()
)
```

**Résultats de l'agrégation:**

| Nom courtier | Type d'affaire | Commissions |
|-------------|----------------|-------------|
| Courtier001 | Affaires différées | 25,013.21 € |
| Courtier001 | Affaires nouvelles commissionnées | 4,281.34 € |
| Courtier001 | Affaires récurrentes | 597.22 € |
| Courtier001 | Reprises de commissions | -3,701.73 € |
| Courtier002 | Affaires récurrentes | 165.63 € |
| Courtier003 | Affaires nouvelles commissionnées | 1,025.77 € |
| Courtier003 | Reprises de commissions | -2,079.07 € |
| Courtier003 | Frais de reversement | 2.30 € |

### 3. Génération automatique de fichiers par courtier

```python
# Création du répertoire de sortie
os.makedirs('./Fichiers', exist_ok=True)

# Génération d'un fichier Excel par courtier
for Nom_courtier in df2['Nom courtier'].unique():
    df_courtier = df2[df2['Nom courtier'] == Nom_courtier]
    
    with pd.ExcelWriter(f'./Fichiers/{Nom_courtier}.xlsx') as writer:
        for Type_affaire in df_courtier['Type d\'affaire'].unique():
            (df_courtier[df_courtier['Type d\'affaire'] == Type_affaire]
             .to_excel(writer, sheet_name=str(Type_affaire)))
```


