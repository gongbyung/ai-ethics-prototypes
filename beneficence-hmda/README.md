# Beneficence: HMDA Rate Spread & Lifetime Harm Estimator  
**Part of:** *AI Ethics Prototypes — Walking the Walk*  
**Author:** Alex Kim  

---

## Goal  
Quantify and interpret **pricing disparities** in U.S. mortgage lending.

This project builds a **proof-of-concept cost fairness pipeline**, combining econometric and machine learning models to examine disparities in loan pricing under the principle of **Beneficence** (*do good, avoid harm*).

It’s not about optimizing profit.  
It’s about measuring **who bears the cost of inequality** in credit markets.

---

## Why This Matters  
Even when approvals look fair, pricing gaps can persist, quietly amplifying inequality over the life of a loan.  
A few basis points can compound into thousands of dollars over a 30-year mortgage.

This project acts as a **cost fairness lens**, estimating lifetime borrower harm (or benefit) from rate disparities across demographic groups.  
It bridges statistical fairness with **ethical impact**, quantifying who benefits, who pays, and by how much.

---

## Project Structure  

```text
beneficence-hmda/
├── data/                  # (gitignored) HMDA data folders
│   ├── raw/               # original downloads (not tracked)
│   ├── interim/           # cleaned/intermediate data
│   └── processed/         # model-ready datasets
│
├── notebooks/
│   ├── 01_eda.ipynb       # exploratory data analysis
│   ├── 02_model.ipynb     # OLS + ML pricing models
│
├── outputs/               # selected figures, tables
├── requirements.txt
├── workflow.png
└── README.md
```

---

## Methodology
1. Exploratory Data Analysis (EDA)
   • Work with approved loans only (action_taken = 1)
   • Inspect rate_spread (APR − APOR) distributions by race/ethnicity, income, and loan type
   • Profile borrower/loan features: LTV, DTI, loan amount, term, occupancy, property type

2. Baseline Pricing Model — OLS
   • Target: rate_spread (APR − APOR)
   • Controls: loan-to-value, debt-to-income, loan amount, term, occupancy, property type
   • Optional: lender and metro fixed effects
   • Group effects: race/ethnicity dummies (White baseline)
   • Goal: Does race predict higher pricing conditional on financial risk?

3. ML Pricing Models — Nonlinear Patterns
   • Algorithms: Random Forest Regressor, XGBoost Regressor
   • Compare model fit (R², RMSE) to OLS
   • Examine feature importances to explore nonlinear effects

4. Lifetime Harm Estimation
   • Compute simple lifetime harm:
       Lifetime_Harm ≈ Δrate × loan_amount × term_years
   • Optional: amortized interest formula for realistic total cost comparison

5. Fairness Readout
   • Summaries by group: mean rate_spread, model-adjusted coefficients, lifetime harm
   • Visuals: density plots, SHAP summary, bar charts by demographic group

---

## Data
Source: CFPB HMDA Public Data
   • Download from https://ffiec.cfpb.gov/data-publication/
   • Only approved loans included (for pricing analysis)
   • rate_spread = APR − APOR (available only for higher-priced loans)
   • Missing values: not zero-filled; analyzed cautiously

---

## Limitations
• HMDA omits credit scores and key underwriting variables → possible omitted variable bias
• Approved-only data → potential selection bias (approval ≠ pricing)
• rate_spread heterogeneity → coverage differs by loan size, points/credits, channel
• MVP keeps model simple; IPW or selection correction optional in later iterations

---

## References
• CFPB HMDA Documentation — rate spread & APOR methodology
• Apgar, Bendimerad, & Essene (2007). "Mortgage Market Channels and Fair Lending"
• Standard amortization formulas — equal-payment, fixed-rate loans

Status: Prototype (MVP)
Author: Alex Kim
