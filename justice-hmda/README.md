# Justice: HMDA Disparate Impact Detector

**Goal:** Identify potential disparate impacts in mortgage lending using Home Mortgage Disclosure Act (HMDA) data.

This project is part of the **AI Ethics Prototypes: Walking the Walk** repository.  
It explores how algorithmic fairness can be tested in lending, using a mix of statistical methods and machine learning.

---

## 📌 Why This Matters
Even after controlling for credit risk factors, research has shown that minority borrowers often face less favorable outcomes in mortgage lending.  
This project provides a **proof-of-concept pipeline** to detect and visualize disparate impacts in loan approvals and pricing.

Think of it as a **fairness smoke detector** — it won’t prove intent, but it will beep when outcomes drift so you can investigate before the house burns down.

---

## 🛠 Project Structure

```text
justice-hmda/
├── data/                  # data storage (gitignored)
│   ├── raw/               # original HMDA data (not tracked in git)
│   ├── interim/           # cleaned & filtered data
│   └── processed/         # model-ready datasets
├── notebooks/             # interactive exploration
│   ├── 01_eda.ipynb       # exploratory data analysis
│   ├── 02_baseline.ipynb  # baseline models (logit/OLS)
│   ├── 03_improved.ipynb  # ML models + fairness metrics
│   └── 04_reporting.ipynb # charts + tables
├── outputs/               # results (gitignored except selected figs/tables)
│   ├── figures/
│   └── tables/
├── src/                   # source code
│   ├── data_prep.py       # cleaning, filtering, feature engineering
│   ├── baseline_models.py # logistic regression, OLS
│   ├── ml_models.py       # RF, XGB, trade-off analysis
│   ├── fairness_metrics.py # disparate impact, equal opp, etc.
│   ├── viz.py             # plotting utilities
│   └── __init__.py
├── tests/                 # basic unit tests
├── requirements.txt       # dependencies
├── workflow.png           # pipeline diagram
└── README.md              # this file

---

## 📊 Methodology

### 1. Exploratory Data Analysis (EDA)
- Distribution of approvals/denials by race, income, loan type  
- Rate spread distributions across groups  
- Geographic breakdowns (metro area dummies)

### 2. Baseline Models
- **Approvals:** Logistic regression  
- **Pricing (Rate Spreads):** OLS regression  
- Report gaps after controlling for borrower + loan characteristics

### 3. Improved Models
- Machine learning (Random Forest, XGBoost)  
- Evaluate trade-offs between **accuracy** and **fairness**  
- Fairness metrics: disparate impact ratio, equal opportunity difference  

### 4. Outputs
- Tables: approval rate gaps, pricing disparities  
- Figures: residual plots, fairness metric dashboards  
- Discussion of **limitations** (HMDA missing variables, selection bias)  
- Recommendations for future research (e.g., bank-specific models, credit score data)

---

## 📂 Data

This project uses **public HMDA data** (download from [CFPB HMDA Portal](https://ffiec.cfpb.gov/data-publication/)).  

- **Not included in repo**: raw HMDA data (too large, gitignored).  
- **Included**: small synthetic/sample dataset in `data/sample/` for reproducibility.  

---

## ⚖️ Limitations

- Public HMDA data does **not include credit scores or detailed underwriting data**, which means omitted variable bias is a risk.  
- This is a **prototype** — results should be seen as exploratory, not regulatory findings.  
- Metro-area and lender-level effects are simplified.  

---

## 📌 References
- Courchane, Nebhut, & Nickerson (2000). *Lessons Learned: Statistical Techniques and Fair Lending*  
- Apgar, Bendimerad, & Essene (2007). *Mortgage Market Channels and Fair Lending*  
- Lee & Floridi (2021). *Algorithmic Fairness in Mortgage Lending*  

---

## 🚀 Next Steps
- Expand fairness metrics (Equalized Odds, Counterfactual Fairness)  
- Explore **comparative file review** with clustering (unsupervised)  
- Incorporate **other CFPB datasets** for context (e.g., complaint database)  
- Package pipeline into a reproducible Python package  

---

**Status:** Prototype (MVP)  
**Author:** Alex Kim  

