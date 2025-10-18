# Who’s Guarding the Consumers?  
**Part of:** *AI Ethics Prototypes — Walking the Walk*  
**Author:** Alex Kim  

---

### Goal  
Detect and interpret potential **consumer harm patterns** in financial services using the **Consumer Financial Protection Bureau (CFPB)** complaint database.  

This project builds a **proof-of-concept NLP pipeline** — combining public complaint narratives, natural language embeddings, and ethical reasoning to identify emerging risks and systemic issues.  

It’s not about replacing the regulator.  
It’s about **scaling its awareness** — listening to consumers when institutions stop listening.

---

## Why This Matters  

The CFPB’s complaint data is one of the richest public signals of financial stress — millions of firsthand accounts of people losing access, losing money, or losing trust.  

But as the Bureau’s active supervision has slowed, those signals risk being lost in the noise.  

This project acts as a **risk radar** for the modern financial system:  
It doesn’t prove intent.  
It surfaces **where to look before the next scandal breaks**.  

---

## Project Structure  

```text
cfpb-nlp-risk-watchdog/
├── data/                      
│   ├── raw/                   # original CFPB complaint export (Mortgage 2024)
│   ├── interim/               # cleaned/intermediate data
│   └── processed/             # (upcoming) model-ready datasets
│
├── notebooks/
│   ├── 01_eda_cfpb_mortgage.ipynb   # exploratory data analysis + cleaning
│   └── 02_modeling_nlp.ipynb        # embeddings, topic modeling, clustering
│
├── scripts/
│   ├── fetch_cfpb_data.py
│   ├── clean_cfpb_data.py
│   └── __init__.py
│
├── outputs/
│   ├── figures/                # selected visualizations
│   ├── topics/                 # model outputs + cluster summaries
│   └── logs/
│
├── requirements.txt
└── README.md
```
---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Clean and standardize CFPB mortgage complaints (2024, ~12K)
- Remove/redact tokens (e.g., “XXXX”), normalize text/casing, standardize company/issue names
- Compute narrative length stats; inspect outliers
- Plot volume by month, issue, and company

### 2. Theme Extraction
- **BERTopic** with **FinBERT embeddings**
- Produce interpretable clusters (e.g., *Escrow errors*, *Payment delays*, *Foreclosure issues*)
- Track topic frequency and growth as early-warning signals

### 3. Risk Labeling
- Binary target from company response:
  - **1 = Risk** → “Closed with monetary relief” or “Closed with non-monetary relief”
  - **0 = No Risk** → all other outcomes

### 4. Risk Classification
- Fine-tune **FinBERT** with **LoRA (PEFT)** for efficient adaptation
- Task: predict `risk_flag` from cleaned narrative
- Split: 80/20 stratified train–test
- Metrics: **Precision**, **Recall**, **F1**, **ROC-AUC**

### 5. Monitoring & Early Warning
- Score new complaints → `risk_score` (0–1) and map to `topic`
- Aggregate by company/issue/geography; surface hotspots
- Dashboard for trend tracking and analyst triage

---

## Data

This project uses **public CFPB complaint data**, retrieved via the official API:

[Consumer Complaint Database](https://www.consumerfinance.gov/data-research/consumer-complaints/)

**Dataset:**  
Mortgage-related complaints, January–December 2024, filtered for narratives (`has_narrative=true`).

| Field | Description |
|--------|-------------|
| `complaint_id` | Unique complaint identifier |
| `date_received` | Date complaint submitted |
| `company` | Financial institution or servicer |
| `issue`, `sub_product` | CFPB-defined categories |
| `clean_text` | Normalized complaint narrative |
| `word_len` | Word count of complaint |
| `company_response` | Outcome label |
| `timely` | Whether company responded on time |
| `state` | Consumer’s U.S. state or territory |

**Not included in repo:** raw downloads (gitignored due to size).  
**Included:** cleaned, reproducible subset for modeling and inspection.

---

## Limitations

- CFPB redactions prevent entity-level linkage (no names, account details).  
- “Timely” responses track procedural compliance, not satisfaction.  
- Complaint counts may reflect company size or consumer awareness, not misconduct prevalence.  
- Focused only on **2024 Mortgage** segment for MVP clarity — multi-product analysis planned for next phase. 

---

## References

- Consumer Financial Protection Bureau (CFPB). *Consumer Complaint Database Documentation.*  
- Grimmer & Stewart (2013). *Text as Data: The Promise and Pitfalls of Automatic Content Analysis.*  
- Blei, Ng, & Jordan (2003). *Latent Dirichlet Allocation.*  
- Grootendorst (2022). *BERTopic: Neural Topic Modeling with Transformers and c-TF-IDF.*  

