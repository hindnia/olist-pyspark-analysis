# Analyse e-commerce avec PySpark — Dataset Olist

Série d'exercices de data engineering réalisés avec **PySpark** sur le dataset public
[Brazilian E-Commerce by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) :
environ 100 000 commandes passées entre 2016 et 2018 sur une marketplace brésilienne.

L'objectif est de pratiquer les transformations Spark les plus courantes en conditions réelles :
contrôle qualité, jointures, agrégations, fonctions de fenêtre, optimisation des jointures
et écriture de données partitionnées.

## Contenu

Tout le travail se trouve dans le notebook
[`notebooks/olist_pyspark_analysis.ipynb`](notebooks/olist_pyspark_analysis.ipynb),
organisé en trois niveaux :

| Partie | Niveau | Ce qui est travaillé |
|---|---|---|
| **Exercice 1** | Débutant | Unicité des clés, filtres, valeurs nulles, cast, `when/otherwise`, `explode`, `row_number` / `rank` |
| **Exercice 2** | Intermédiaire | Jointures multi-tables, CA et panier moyen par vendeur, délais et retards de livraison, broadcast join vs sort merge join, export Parquet |
| **Exercice 3** | Avancé | Délai de réachat (`lag`), analyse de Pareto avec cumul sur fenêtre, pivot, partitionnement par État, détection d'outliers (3σ), `collect_list`, export JSON gzip partitionné |

Chaque question suit la même démarche : explorer, transformer, puis **vérifier le résultat**
(comptages avant/après, sommes de contrôle, cas limites).

## Quelques résultats

- 99 441 commandes pour **96 096 clients réels** : `customer_id` identifie une commande, pas un client.
- Environ **8 %** des commandes livrées arrivent après la date estimée.
- Seulement **3,1 %** des clients repassent commande, en moyenne 80 jours après la première.
- **45 % des clients** concentrent 80 % du chiffre d'affaires.
- Pic d'activité en **novembre 2017** (Black Friday) ; la carte de crédit domine les paiements.
- Le classement des villes par frais de port moyens change complètement une fois les villes à très faible effectif écartées.

## Données

Les 9 tables utilisées :

```
customers · orders · order_items · order_payments · order_reviews
products · sellers · geolocation · product_category_name_translation
```

Le dataset n'est **pas inclus** dans le dépôt : il est téléchargé à l'exécution via l'API Kaggle.
Il est publié sous licence CC BY-NC-SA 4.0.

## Exécuter le notebook

### Sur Google Colab (recommandé)

1. Ouvrir le notebook dans Colab (`Fichier > Ouvrir un notebook > GitHub`).
2. Générer une clé API Kaggle : *Kaggle > Settings > API > Create New Token* (télécharge `kaggle.json`).
3. Exécuter les cellules dans l'ordre ; la troisième demande d'uploader `kaggle.json`.

### En local

Prérequis : Python 3.9+ et Java 8/11/17 (nécessaire à Spark).

```bash
git clone https://github.com/<votre-utilisateur>/olist-pyspark-analysis.git
cd olist-pyspark-analysis
python -m venv .venv
source .venv/bin/activate        # Windows : .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/olist_pyspark_analysis.ipynb
```

> Le notebook a été écrit pour Colab : la cellule d'upload utilise `google.colab.files`.
> En local, placer `kaggle.json` dans `~/.kaggle/` et ne pas exécuter cette cellule.

## Structure du projet

```
olist-pyspark-analysis/
├── notebooks/
│   └── olist_pyspark_analysis.ipynb   # exercices commentés avec leurs résultats
├── .gitignore                          # exclut données, sorties Spark et clé Kaggle
├── requirements.txt
└── README.md
```

Les dossiers `olist_data/` (CSV bruts) et `output/` (Parquet et JSON générés) sont créés
à l'exécution et ne sont pas versionnés.

## Stack

PySpark · Spark SQL · Python · Google Colab · Parquet · JSON

## Auteur

**Hind Nia** — Data Engineer
