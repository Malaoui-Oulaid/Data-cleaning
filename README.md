# 🧹 IACamp1: Data Engineering & Cleaning Pipeline

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data_Manipulation-150458?style=flat&logo=pandas)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📋 Project Overview

This project represents the data preparation phase for the **IACamp1 Competition**. The objective was to consolidate disparate, messy, and obfuscated data sources into a single, high-quality dataset suitable for machine learning analysis.

The raw data was split into **4 subsets**, each presenting unique quality challenges ranging from column name obfuscation to inconsistent unit measurements and embedded textual information.

## 🏗️ The Challenge

The training data was fragmented into four files (`train_subset_1` to `train_subset_4`), each with specific issues:
* **Obfuscated Metadata:** Columns in subsets 2 and 3 had hashed names (e.g., `z153yi8`, `u5erb551yo`) requiring mapping.
* **Inconsistent Formatting:** Numerical values mixed with text (e.g., "6.36 Millions", "$42K+", "10K or more").
* **Dirty Categorical Data:** Typos in categories (e.g., `sUPermARkEt`, `Sao PauLo`).
* **Embedded Information:** Key demographic and transaction details were buried in natural language strings (e.g., *"Single Male with three children..."*).
* **Unit Mismatches:** Weights mixed between kilograms, grams, and pounds.

## ⚙️ The Pipeline

The solution is divided into two main stages:

### 1. Data Ingestion & Harmonization (`IACamp1.ipynb`)
* **Loading:** Ingested the 4 training subsets and the test set.
* **Schema Alignment:** Identified and renamed obfuscated columns in subsets 2, 3, and 4 to match the master schema.
* **Parsing Complex Columns:** Extracted mixed values (e.g., splitting a combined `weights` column into `gross` and `net`).
* **Merging:** Concatenated all subsets into a unified `train.csv`.

### 2. Advanced Data Cleaning & Feature Engineering (`iacamp1.ipynb`)
This notebook performs deep cleaning using **RegEx** and **Pandas**:

* **Regex Parsing:** * Standardized `yearly_income` and `store_sales` (converting "K", "Millions", and currency symbols to floats).
    * Cleaned geographical coordinates (`lat`/`lon`) and city names.
    * Normalized `review_score` (handling formats like "5/5", "100%", "4.0").
* **Unit Standardization:** Converted all weight columns to Kilograms (handling 'lb', 'grams').
* **Text Normalization:** Fixed Typos in `store_kind` (e.g., "Deluxe", "Supermarket") using fuzzy logic and pattern matching.
* **Feature Engineering:**
    * **Demographics:** Parsed `person_description` to extract `Marital Status`, `Gender`, `Children Count`, `Education`, and `Job Type`.
    * **Product Details:** Parsed `customer_order` to extract `Product Category`, `Department`, and `Brand`.
* **Imputation:** Calculated missing `package_weight` using the logic: $|Gross - Net|$.

## 📊 Results

| Feature | Raw Input Example | Final Output |
| :--- | :--- | :--- |
| **Sales** | `$6.36 Millions` | `6360000.0` |
| **Demographics** | `Single Female with 3 kids...` | `Single` (Status), `Female` (Gender), `3` (Kids) |
| **Store Type** | `sUPermARkEt` | `Supermarket` |
| **Weights** | `1453.60 grams` | `1.4536` (kg) |

## 🚀 How to Run

1. Clone the repository:
   ```bash
   git clone [https://github.com/YOUR_USERNAME/IACamp1-Data-Cleaning.git](https://github.com/YOUR_USERNAME/IACamp1-Data-Cleaning.git)
