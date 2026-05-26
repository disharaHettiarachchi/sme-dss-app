<div align="center">

# 🤖 AI-Driven Autonomous DSS for SME Analytics

### Bringing enterprise-grade business intelligence to small and medium enterprises

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.39-FF4B4B?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.6-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.18-FF6F00?logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Dataset: CC BY 4.0](https://img.shields.io/badge/Dataset-CC%20BY%204.0-lightgrey)](https://archive.ics.uci.edu/dataset/502/online+retail+ii)

[**🚀 Live Demo**](https://YOUR-APP-NAME.streamlit.app) · [**📄 Research Paper**](docs/Full_Research_Paper.pdf) · [**📓 Experiments**](AI_DSS_SME_Experiments.ipynb)

</div>

---

## 🎯 The Problem

> Small and medium enterprises (SMEs) make up **90%+ of all businesses worldwide** and generate huge volumes of transactional data — yet **fewer than 20%** apply advanced analytics to inform their strategic decisions.

Traditional BI tools (Tableau, Power BI, etc.) demand technical expertise, dedicated analysts, and infrastructure most SMEs cannot afford. The result is a structural analytics gap: SMEs *generate* data but can't *act* on it.

## 💡 The Solution

An **autonomous, AI-driven Decision Support System** that runs three analytical pipelines without any model configuration from the user:

| Capability | Algorithm | What it answers |
|---|---|---|
| 👥 **Customer Segmentation** | k-Means on RFM | _Who are my best customers? Who's at risk?_ |
| 🔮 **Sales Forecasting** | Random Forest · Gradient Boosting · LSTM | _What's next week's revenue likely to be?_ |
| ⚠️ **Anomaly Detection** | Isolation Forest | _Which transactions need a closer look?_ |

The system ingests raw transactional CSV data and outputs plain-language recommendations — no statistics interpretation required from the SME operator.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│   Layer 6 — User Interface (Streamlit Dashboard)        │
├─────────────────────────────────────────────────────────┤
│   Layer 5 — Autonomous Decision Engine                  │
│             (Rule-based + AI recommendations)           │
├─────────────────────────────────────────────────────────┤
│   Layer 4 — Analytics & Insight                         │
│             (Descriptive · Predictive · Prescriptive)   │
├─────────────────────────────────────────────────────────┤
│   Layer 3 — Machine Learning Engine                     │
│             (k-Means · RF · GB · LSTM · iForest)        │
├─────────────────────────────────────────────────────────┤
│   Layer 2 — Data Processing                             │
│             (Cleaning · RFM · Temporal features)        │
├─────────────────────────────────────────────────────────┤
│   Layer 1 — Data Ingestion (CSV/Excel upload)           │
└─────────────────────────────────────────────────────────┘
```

---

## 📊 Empirical Results

Validated on the **UCI Online Retail II** dataset — 1.07 M real transactions from a UK-based SME (2009–2011).

### Customer Segmentation
- **5,819 customers** auto-grouped into **4 segments**
- Silhouette score: **0.361** · Davies-Bouldin index: **0.914**

| Segment | Customers | Avg Recency | Avg Frequency | Avg Monetary |
|---|---:|---:|---:|---:|
| 🏆 Champions | 1,080 | 44 days | 16.9 orders | £7,028 |
| 💎 Potential Loyalists (high-value) | 1,853 | 113 days | 5.0 orders | £1,710 |
| 🌱 Potential Loyalists (new) | 1,316 | 108 days | 1.7 orders | £407 |
| ⚠️ At-Risk / Hibernating | 1,570 | 498 days | 1.6 orders | £497 |

### Sales Forecasting

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| 🥇 **Random Forest** | **£13,127** | **£20,060** | **0.164** |
| Gradient Boosting | £13,148 | £20,291 | 0.144 |
| LSTM | £14,002 | £21,752 | 0.030 |

> _Interpretation: Random Forest wins narrowly. The modest R² reflects the inherent volatility of daily SME revenue — forecasts are best used as a directional aid, not a hard target._

### Anomaly Detection
- 200,000 transactions analysed · **1,972 flagged** (0.99%)
- 19,494 cancellation records identified for separate review

---

## 🚀 Quick Start

### Option 1 — Use the live app
Just visit the [**deployed dashboard**](https://YOUR-APP-NAME.streamlit.app), no setup needed. Upload your own CSV on the "Upload Your Data" page.

### Option 2 — Run locally

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/sme-dss-app.git
cd sme-dss-app

# Set up environment (Python 3.11 recommended)
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Launch the dashboard
streamlit run app.py
```

App opens at `http://localhost:8501`.

### Option 3 — Reproduce the experiments

```bash
# 1. Download the dataset (44 MB)
#    https://archive.ics.uci.edu/dataset/502/online+retail+ii
#    Save online_retail_II.xlsx to the project root.

# 2. Open the notebook in Google Colab or Jupyter
jupyter notebook AI_DSS_SME_Experiments.ipynb

# 3. Run all cells. Generates outputs/ directory with trained models.
```

---

## 📁 Project Structure

```
sme-dss-app/
├── app.py                              # Streamlit dashboard (6 pages)
├── AI_DSS_SME_Experiments.ipynb        # Full ML pipeline notebook
├── requirements.txt                    # Pinned dependencies
├── runtime.txt                         # Python 3.11 lock for Streamlit Cloud
├── .streamlit/
│   └── config.toml                     # Theme & server config
├── outputs/                            # Pre-trained artifacts (auto-loaded)
│   ├── models/                         # .joblib + .keras files
│   ├── figures/                        # Generated plots
│   ├── rfm_segments.csv                # Per-customer segment assignments
│   ├── cluster_profile.csv             # Segment summary statistics
│   ├── segmentation_metrics.json       # k-Means evaluation metrics
│   ├── forecasting_metrics.json        # RF/GB/LSTM performance
│   └── anomaly_metrics.json            # Isolation Forest summary
└── README.md                           # You are here
```

---

## 🧪 Methodology

This project follows the **Design Science Research (DSR)** paradigm of Hevner et al. (2004), structured into six iterative phases:

```
1. Literature Review  →  2. Requirements  →  3. Framework Design
       ↓                                              ↓
6. Synthesis  ←  5. Evaluation  ←  4. Prototype Development
```

The full methodology is documented in **Chapter 3** of the [research paper](docs/Full_Research_Paper.pdf).

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| **Data** | pandas · NumPy · openpyxl |
| **ML — Classical** | scikit-learn (k-Means, Random Forest, Gradient Boosting, Isolation Forest) |
| **ML — Deep Learning** | TensorFlow / Keras (LSTM) |
| **Visualisation** | Matplotlib · Seaborn |
| **App framework** | Streamlit |
| **Persistence** | joblib · native Keras `.keras` format |
| **Deployment** | Streamlit Community Cloud |

---

## 📜 Citation

If you reference this work in academic publications, please cite:

```bibtex
@misc{hettiarachchi2026smedss,
  title  = {Designing an AI-Driven Autonomous Decision Support System
            for Small and Medium Enterprise Analytics},
  author = {Hettiarachchi, D. W. M. M.},
  year   = {2026},
  note   = {MSc Research Report, MITS Advanced Research Techniques}
}
```

Dataset citation:

```bibtex
@misc{chen2012onlineretail,
  title     = {Online Retail II [Dataset]},
  author    = {Chen, Daqing},
  year      = {2012},
  publisher = {UCI Machine Learning Repository},
  doi       = {10.24432/C5CG6D}
}
```

---

## 🔬 Limitations & Future Work

This is a **prototype**, not a production system:

- ✗ Trained on a single SME's data (UK gift retailer) — generalisation across industries needs further validation
- ✗ Models are batch-trained, not continuously learning
- ✗ No authentication or multi-tenant support
- ✗ Explainability is segment-level, not transaction-level

**Planned next steps:**
- [ ] Add SHAP-based per-prediction explanations
- [ ] Industry-specific model variants (retail, services, manufacturing)
- [ ] Scheduled retraining pipeline
- [ ] Multi-language UI (English / Sinhala / Tamil)

---

## 📄 License

- **Code:** MIT — see [LICENSE](LICENSE)
- **Research paper:** © 2026 D. W. M. M. Hettiarachchi. All rights reserved.
- **Dataset:** Creative Commons Attribution 4.0 International — Chen (2012), UCI ML Repository

---

<div align="center">

**Built with ❤️ as part of an MSc research project in Business Analytics.**

[⬆ back to top](#-ai-driven-autonomous-dss-for-sme-analytics)

</div>
