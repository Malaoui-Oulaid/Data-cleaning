#  IACamp1 : Pipeline de Data Engineering & Nettoyage des Données

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data_Manipulation-150458?style=flat&logo=pandas)
![Status](https://img.shields.io/badge/Status-Terminé-success)

##  Aperçu du Projet

Ce projet constitue la phase de préparation des données pour la compétition **IACamp1**. L'objectif était de consolider des sources de données disparates, désordonnées et obfusquées (masquées) en un jeu de données unique, propre et de haute qualité, prêt pour l'analyse et le Machine Learning.

Les données brutes étaient réparties en **4 sous-ensembles (subsets)**, chacun présentant des défis de qualité uniques, allant de l'obfuscation des noms de colonnes à des incohérences d'unités et des informations textuelles imbriquées.

##  Le Défi

Les données d'entraînement étaient fragmentées en quatre fichiers (`train_subset_1` à `train_subset_4`), avec des problèmes spécifiques :
* **Métadonnées Obfusquées :** Les colonnes des subsets 2 et 3 avaient des noms hachés (ex: `z153yi8`, `u5erb551yo`) nécessitant un mappage.
* **Formatage Incohérent :** Mélange de valeurs numériques et textuelles (ex: "6.36 Millions", "$42K+", "10K or more").
* **Données Catégorielles "Sales" :** Fautes de frappe et variations de casse (ex: `sUPermARkEt`, `Sao PauLo`).
* **Informations Imbriquées :** Des détails démographiques et transactionnels clés étaient enfouis dans des chaînes de langage naturel (ex: *"Single Male with three children..."*).
* **Incohérence des Unités :** Mélange de poids en kilogrammes, grammes et livres (lb).

##  Le Pipeline

La solution est divisée en deux étapes principales :

### 1. Ingestion & Harmonisation des Données (`01_Data_Merging.ipynb`)
* **Chargement :** Importation des 4 sous-ensembles d'entraînement et du jeu de test.
* **Alignement de Schéma :** Identification et renommage des colonnes obfusquées dans les subsets 2, 3 et 4 pour correspondre au schéma maître.
* **Parsing de Colonnes Complexes :** Extraction de valeurs mixtes (ex: séparation d'une colonne `weights` combinée en `gross` et `net`).
* **Fusion :** Concaténation de tous les sous-ensembles en un fichier unifié `train.csv`.

### 2. Nettoyage Avancé & Feature Engineering (`02_Data_Cleaning_and_Feature_Engineering.ipynb`)
Ce notebook effectue un nettoyage en profondeur utilisant des **Regex** (Expressions Régulières) et **Pandas** :

* **Parsing Regex :**
    * Standardisation de `yearly_income` et `store_sales` (conversion des "K", "Millions" et symboles monétaires en flottants).
    * Nettoyage des coordonnées géographiques (`lat`/`lon`) et des noms de villes.
    * Normalisation du `review_score` (gestion des formats comme "5/5", "100%", "4.0").
* **Standardisation des Unités :** Conversion de toutes les colonnes de poids en Kilogrammes (gestion des 'lb', 'grams').
* **Normalisation de Texte :** Correction des fautes dans `store_kind` (ex: "Deluxe", "Supermarket") via logique floue et reconnaissance de motifs.
* **Feature Engineering (Ingénierie des fonctionnalités) :**
    * **Démographie :** Parsing de `person_description` pour extraire : `État Civil`, `Genre`, `Nombre d'enfants`, `Éducation` et `Type d'emploi`.
    * **Détails Produits :** Parsing de `customer_order` pour extraire : `Catégorie Produit`, `Département` et `Marque`.
* **Imputation :** Calcul des valeurs manquantes pour `package_weight` selon la logique : $|Brut - Net|$.

##  Résultats

| Fonctionnalité | Exemple Entrée Brute | Sortie Finale |
| :--- | :--- | :--- |
| **Ventes (Sales)** | `$6.36 Millions` | `6360000.0` |
| **Démographie** | `Single Female with 3 kids...` | `Single` (Statut), `Female` (Genre), `3` (Enfants) |
| **Type Magasin** | `sUPermARkEt` | `Supermarket` |
| **Poids** | `1453.60 grams` | `1.4536` (kg) |
