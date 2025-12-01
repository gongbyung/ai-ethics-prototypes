# Who’s Guarding the Consumers?  

**Author:** Alex Kim & Patrick Abousleiman

---

### Goal  
Build an NLP pipeline that detects potential consumer harm in financial services using the CFPB Complaint Database.
We classify complaint narratives into risky vs. non-risky outcomes using company relief responses (monetary or non-monetary) as a proxy label.

This project is a proof-of-concept exploring whether modern NLP methods can serve as a risk radar in an environment where regulatory supervision has weakened.

---

## Why This Matters  

CFPB complaints are one of the most detailed public signals of consumer harm.
But with the CFPB’s supervisory slowdown, millions of narratives risk becoming unread early warnings.

This project does not replace regulation.
Can NLP surface emerging risk patterns before the next consumer-harm event?

---

## Project Structure  

```text
respect-cfpb/
├── data/
│   ├── raw/                 # original CFPB complaints
│   ├── interim/             # cleaned text, normalized fields
│   └── processed/           # train/val/test splits
│
├── notebooks/
│   ├── 01_eda.ipynb                 # cleaning, EDA
│   ├── 02_modeling_nlp.ipynb        # baseline + FinBERT 
│   ├── 02_modeling_nlp.ipynb        # BERT + RoBERTa
│   └── 03_modeling_topic.ipynb      # Post-hoc clustering + error analysis
│
├── outputs/                # saved model checkpoints & metrics
├── report/					# final report
└── README.md
```
---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Filter CFPB mortgage complaints with narratives
- Normalize casing, remove redacted tokens (XXXX), strip whitespace
- Preserve domain-specific mortgage terms
- Examine class imbalance (~5% risky)
- Inspect narrative length distribution (many > 512 tokens)
- Stratified 60/20/20 split into train/validation/test

---

### 2. Baseline Model — TF-IDF + Logistic Regression
A transparent lexical baseline to evaluate whether transformers are worth the added complexity.
- Vectorizer: unigram TF-IDF
- Classifier: Logistic Regression with class_weight='balanced'
- Metrics: Precision, Recall, F1, ROC-AUC

---

### 3. Transformer Models
3.1 Full FinBERT Fine-Tuning

3.2 RoBERTa Fine-Tuning 

3.3 Post-Hoc Topic Modeling (Error Analysis)

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
- Outcome labels (relief vs no relief) are only imperfect proxies for consumer harm
- Redacted data limits entity-level linkage
- Complaint volume reflects consumer awareness, not misconduct frequency

---

## References

- Bastani et al. (2019) Topic modeling CFPB complaints using LDA
- Oyewola et al. (2023) Deep learning for CFPB complaint classification
- Vasudeva Raju et al. (2022) FinBERT for financial text
- Roumeliotis et al. (2025) Reasoning LLMs for complaint classification
- Hu et al. (2021) LoRA parameter-efficient tuning

