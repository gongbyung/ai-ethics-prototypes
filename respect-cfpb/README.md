# Who’s Guarding the Consumers?  
**Part of:** *AI Ethics Prototypes — Walking the Walk*  
**Author:** Alex Kim & Patrick Abousleiman

---

### Goal  
Detect and interpret potential **consumer harm patterns** in financial services using the **Consumer Financial Protection Bureau (CFPB)** complaint database.  

This project develops a **proof-of-concept NLP pipeline** that combines public complaint narratives, natural language embeddings, and ethical reasoning to identify emerging risks and systemic issues.  

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
respect-cfpb/
├── data/                      
│   ├── raw/                   # original CFPB complaint export (Mortgage 2024)
│   ├── interim/               # cleaned/intermediate data
│   └── processed/             # (upcoming) model-ready datasets
│
├── notebooks/
│   ├── 01_eda_cfpb_mortgage.ipynb   # exploratory data analysis + cleaning
│   └── 02_modeling_nlp.ipynb        # embeddings, topic modeling, clustering
│
├── requirements.txt
└── README.md
```
---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Clean and standardize CFPB **mortgage complaints (2024, ~12K)**  
- Remove redacted tokens (e.g., “XXXX”), normalize casing and whitespace  
- Standardize categorical fields (company, issue, sub-product)  
- Compute narrative length statistics and inspect outliers  
- Visualize complaint volume by **month**, **issue**, and **company**

---

### 2. Baseline Model — TF-IDF + Logistic Regression
The first benchmark for complaint-risk detection uses a traditional bag-of-words approach.  
Each complaint narrative is vectorized using **TF-IDF (Term Frequency–Inverse Document Frequency)** and fed into a **Logistic Regression** classifier to predict whether the complaint is likely to involve consumer harm.

- Input: `clean_text` (narrative), `risk_flag` (binary target)  
- Evaluation: Precision, Recall, F1-Score, ROC-AUC  
- Purpose: Establish an interpretable baseline before transformer fine-tuning

---

### 3. Theme Extraction
- Use **BERTopic** with **FinBERT embeddings** to extract latent complaint themes  
- Generate interpretable clusters (e.g., *Escrow Errors*, *Payment Delays*, *Foreclosure Issues*)  
- Track **topic frequency and growth** as early-warning signals for emerging consumer risk  
- Compare across companies and time periods to identify systemic issues

---

### 4. Risk Labeling
- Construct a binary target from company responses:  
  - **1 = Risk** → “Closed with monetary relief” or “Closed with non-monetary relief”  
  - **0 = No Risk** → all other outcomes  
- This label serves as the ground truth for supervised training and validation

---

### 5. Risk Classification — FinBERT + LoRA Fine-Tuning
To capture nuanced language patterns in financial complaints, the model fine-tunes **FinBERT**, a domain-specific variant of BERT trained on financial text.

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
- Complaint counts may reflect company size or consumer awareness, not misconduct prevalence.  

---

## References

- Consumer Financial Protection Bureau (CFPB). *Consumer Complaint Database Documentation.*  
- Grimmer & Stewart (2013). *Text as Data: The Promise and Pitfalls of Automatic Content Analysis.*  
- Blei, Ng, & Jordan (2003). *Latent Dirichlet Allocation.*  
- Grootendorst (2022). *BERTopic: Neural Topic Modeling with Transformers and c-TF-IDF.*  

