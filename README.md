# AML Transaction Monitoring — XplainTM

**MSc Financial Technology — Practicum / Applied Project**  
*National College of Ireland, Dublin | Student ID: 24198170*

---

## Overview

XplainTM is an anti-money laundering (AML) transaction monitoring project that builds a cost-sensitive, explainable machine learning pipeline on top of the IBM AML HI-Small transaction dataset. The goal is to reduce false positives and improve detection of true laundering cases at a fixed alert capacity, compared with traditional rules-based monitoring.

The project compares a typology-based rules engine, a logistic regression baseline and a cost-sensitive XGBoost model, and then uses SHAP-based reason codes to make alerts more transparent for compliance analysts.

---

## Objectives

- Use the IBM AML HI-Small dataset with `Is_Laundering` as the target label for suspicious transactions
- Apply a **temporal train/validation/test split** to avoid look-ahead bias and leakage
- Engineer behavioural features that capture AML typologies such as velocity, structuring, fan-in and fan-out
- Build and calibrate a typology-based rules engine as a non-ML baseline
- Train a logistic regression model as a simple ML benchmark
- Train cost-sensitive XGBoost models using class weighting and/or SMOTE resampling
- Generate SHAP explanations and map them into concise, consistent **reason codes** for each alert
- Evaluate models using metrics aligned with AML operations rather than only generic accuracy

---

## Dataset

- Source: IBM Transactions for Anti-Money Laundering (HI-Small)
- Size: ~500,000 synthetic transactions with about 0.12% labelled as money laundering
- Label: `Is_Laundering` (binary target)
- Characteristics:
  - Highly imbalanced classification problem
  - Transactions associated with one or more of 38 money laundering patterns
  - Temporal ordering used for splitting and feature construction

The dataset is synthetic and publicly available, which avoids customer privacy concerns and supports reproducible research.

---

## Methods & Models

- **Data splitting and setup**
  - Temporal split into train (70%), validation (15%) and test (15%) sets
  - Rolling-window feature construction to avoid future information in past features

- **Feature engineering**
  - Velocity features: counts and sums of transactions over recent time windows
  - Structuring proxies: repeated near-threshold amounts just under reporting limits
  - Fan-in / fan-out: number of counterparties sending to and receiving from an account
  - Timing features: unusual transaction times relative to account history

- **Baselines**
  - Typology-based rules engine (velocity, structuring, fan-in, fan-out) calibrated to match alert volumes
  - Logistic regression as a simple ML benchmark

- **Cost-sensitive modelling**
  - XGBoost models with:
    - Standard training
    - Class weighting via `scale_pos_weight`
    - SMOTE oversampling to address class imbalance

- **Explainability and reason codes**
  - TreeSHAP to compute local feature attributions for each alert
  - Mapping top SHAP drivers to a compact, human-readable set of reason codes, such as:
    - High transaction velocity in a short time window
    - Structuring: repeated transactions just under a reporting threshold
    - Unusual counterparties or directions of flow

---

## Evaluation

Model performance is evaluated using metrics that reflect AML operations:

- Precision–Recall AUC (PR-AUC), which is more appropriate than ROC-AUC under extreme class imbalance
- Recall of true laundering cases within the top 0.5%, 1% and 5% of ranked alerts
- False positives per true positive at a fixed number of alerts (analyst capacity)
- Consistency of reason codes across alerts of the same typology
- Inference runtime to ensure the pipeline can run on a realistic daily or overnight schedule

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Python 3** | Core programming language |
| **Pandas / NumPy** | Data processing and feature engineering |
| **scikit-learn** | Baseline models, splitting, metrics |
| **XGBoost** | Cost-sensitive gradient boosting for AML classification |
| **imbalanced-learn (SMOTE)** | Handling severe class imbalance |
| **SHAP** | Model explanation and feature attribution |
| **LIME** | Additional local explainability checks |
| **Matplotlib / Seaborn** | Visualisation of distributions, metrics and explanations |
| **Jupyter Notebook** | End-to-end experimentation and documentation |

---

## How to Run

1. Clone this repository or download the files
2. Open the main AML pipeline notebook in **Jupyter Notebook** or **Google Colab**
3. Ensure the IBM AML dataset CSV files are available in the expected data folder
4. Run all cells sequentially to reproduce feature engineering, model training, evaluation and explanation steps

> **Requirements:** Python 3.x, pandas, numpy, scikit-learn, xgboost, imbalanced-learn, shap, lime, matplotlib, seaborn

---

## Author

**Mohd Nizam Nasir Shaikh**

MSc Financial Technology — National College of Ireland, Dublin

LinkedIn: https://www.linkedin.com/in/nizam-shaikh-90b737199  
GitHub: https://github.com/nizamshaikh12
