# Justice: HMDA Disparate Impact Detector  
**Part of:** *AI Ethics Prototypes — Walking the Walk*  
**Author:** Alex Kim  

---

### Goal  
Detect and interpret potential **disparate impacts** in U.S. mortgage lending using Home Mortgage Disclosure Act (HMDA) data.  

This project builds a **proof-of-concept fairness pipeline** — combining traditional econometrics, machine learning, and ethical reasoning via large language models.  

It’s not about predicting who gets a loan.  
It’s about understanding **whether the system treats similar borrowers equally**.  

---

## Why This Matters  
Even after controlling for income, debt, and loan size, decades of research show that minority borrowers often face lower approval rates or higher costs.  

This project acts as a **fairness smoke detector** — it doesn’t prove intent, but it alerts you when outcomes drift, so you can investigate before the house burns down.  

---

## Project Structure  

```text
justice-hmda/
├── data/                  # (gitignored) HMDA data folders
│   ├── raw/               # original downloads (not tracked)
│   ├── interim/           # cleaned/intermediate data
│   └── processed/         # model-ready datasets
│
├── notebooks/
│   ├── 01_eda.ipynb       # exploratory data analysis
│   ├── 02_baseline.ipynb  # logistic fairness models
│   ├── 03_improved.ipynb  # ML models (RF/XGB) + fairness tests
│   ├── fairness_prompt_context.txt
│   ├── fairness_comparison_summary.txt
│   └── fairness_results.json
│
├── outputs/               # selected figures, tables
├── src/
│   ├── data_prep.py
│   ├── baseline_models.py
│   ├── ml_models.py
│   ├── fairness_metrics.py
│   ├── viz.py
│   └── __init__.py
│
├── tests/
├── requirements.txt
├── workflow.png
└── README.md
```
---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Distribution of approvals/denials by race and income
- Rate-spread and loan-to-value histograms
- County-level loan patterns (proxy for redlining)

### 2. Baseline Models
- **Logistic regression** for approval probability  
- Controls: income ratio, loan-to-value (LTV), debt-to-income (DTI)
- Race-group indicators (White baseline) to estimate conditional approval disparities  

### 3. Improved Models
- **Random Forest** and **XGBoost** for nonlinear fairness benchmarking  
- Evaluate fairness trade-offs 
- Metrics: Disparate Impact (DI), False Negative Rate (FNR), False Positive Rate (FPR)

### 4. LLM Fairness Layer (Prototype)
- Outputs from models are summarized and sent to a large language model (via OpenRouter API)
- The LLM acts as an “AI Ethics Officer,” interpreting fairness results through the **Belmont Report** principles:  
   - Respect for Persons  
   - Beneficence  
   - Justice  

---

## Data

This project uses **public HMDA data** (download from [CFPB HMDA Portal](https://ffiec.cfpb.gov/data-publication/)).  

- **Not included in repo**: raw HMDA data (too large, gitignored).  
- **Included**: sample/synthetic datasets for reproducibility.

---

## Limitations

- Public HMDA data lacks credit scores and detailed underwriting variables.
- Omitted variable bias and selection effects are likely.
- Results are **illustrative**, not regulatory findings.
- Geographic and lender fixed effects simplified for MVP clarity.

---

## Example Results Snapshot  

Below is an illustrative summary of fairness metrics generated from the 2022 California HMDA sample.  
Approval rates are shown by racial group, along with Disparate Impact (DI) ratios, and group-specific error rates from the model predictions.

| Race Group | Approval Rate | Disparate Impact (vs White) | FNR (False Negative Rate) | FPR (False Positive Rate) |
|-------------|---------------|-----------------------------|----------------------------|----------------------------|
| White | 0.851 | 1.00 | 0.087 | 0.468 |
| Asian | 0.842 | 0.99 | 0.111 | 0.312 |
| Hispanic or Latino | 0.815 | 0.96 | 0.149 | 0.406 |
| Black or African American | 0.671 | **0.79 ⚠️** | 0.296 | 0.303 |
| Pacific Islander | 0.783 | 0.92 | 0.143 | 0.190 |
| American Indian or Alaska Native | 0.798 | 0.94 | 0.111 | 0.250 |

⚠️ *Approval ratios below 0.80 may indicate potential disparate impact under the Four-Fifths Rule.*

**Interpretation:**
- The **Disparate Impact ratio (DI)** compares each group’s approval rate to that of White applicants.  
- A DI below 0.80 suggests that a protected class may be disproportionately denied approvals.  
- **FNR** highlights who is more likely to be incorrectly denied (false negatives).  
- **FPR** captures inconsistent or lenient approvals (false positives).  

These metrics together provide a **multidimensional fairness view**, not just who gets approved, but how consistently decisions are made across groups.

---

## References
- Courchane, Nebhut, & Nickerson (2000). *Lessons Learned: Statistical Techniques and Fair Lending*  
- Apgar, Bendimerad, & Essene (2007). *Mortgage Market Channels and Fair Lending*  
- Lee & Floridi (2021). *Algorithmic Fairness in Mortgage Lending*  

---

**Status:** Prototype (MVP)  
**Author:** Alex Kim  



