# 📊 FinRisk-ML — Financial Risk Intelligence Suite

> **Machine learning models and quantitative analysis for corporate default prediction and equity risk profiling — built for venture capital credit screening and portfolio management.**

---

## 🗂️ Repository Overview

This repository contains two end-to-end data science projects developed as part of a Financial Health Assessment Tool for a venture capital firm. Together they form a complete financial risk intelligence pipeline — predicting **corporate insolvency** from balance-sheet data and profiling **market risk** from historical equity prices.

| Project | Domain | Technique | Dataset |
|---|---|---|---|
| **Part A — Financial Default Prediction** | Credit Risk | Logistic Regression, Random Forest | 4,256 companies × 51 financial features |
| **Part B — Stock Market Risk Analysis** | Market Risk | Returns Analysis, Risk-Return Profiling | 5 Indian equities × 418 weekly observations |

---

## 📁 Repository Structure

```
FinRisk-ML/
│
├── README.md                          ← You are here
├── requirements.txt                   ← Python dependencies
├── .gitignore                         ← Files excluded from version control
│
├── part_a/                            ← Financial Default Prediction
│   ├── PartA_Financial_Default_Prediction.ipynb   ← Main notebook
│   └── README_PartA.md                ← Part A detailed guide
│
├── part_b/                            ← Stock Market Risk Analysis
│   ├── PartB_Stock_Market_Risk_Analysis.ipynb     ← Main notebook
│   └── README_PartB.md                ← Part B detailed guide
│
├── data/
│   ├── Comp_Fin_Data.csv              ← Part A dataset (4,256 companies)
│   ├── Market_Risk_Data_coded.csv     ← Part B dataset (418 weekly prices)
│   └── DATA_DICTIONARY.md             ← Full variable descriptions
│
└── reports/
    ├── Financial_Default_Report.pptx  ← Part A business presentation
    └── Stock_Market_Risk_Analysis_Report.pptx  ← Part B business presentation
```

---

## 🧠 Part A — Financial Default Prediction

### Problem Statement
A group of venture capitalists needs to identify which companies in their portfolio are likely to become **financial defaulters** — defined as having a negative net worth in the following fiscal year. Early identification enables proactive risk mitigation before credit losses materialise.

### Approach

```
Raw Data (4,256 companies)
    ↓
EDA — Missing values, distributions, correlation analysis
    ↓
Preprocessing — Drop high-missingness cols, winsorise outliers,
                stratified train/test split, StandardScaler
    ↓
Target creation — default = 1 if Networth Next Year < 0
    ↓
VIF Analysis — Remove multicollinear features (20 features removed)
    ↓
Model 1: Logistic Regression (base + VIF + optimal threshold)
Model 2: Random Forest (base + RandomizedSearchCV tuning)
    ↓
Final Model: Tuned Random Forest
```

### Results

| Model | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---|---|---|---|---|---|
| LR Base | 97.61% | 37.50% | 100.00% | 54.55% | 99.84% |
| LR + VIF + Optimal Threshold | 99.52% | 75.00% | **100.00%** | **85.71%** | **99.84%** |
| RF Base | 99.04% | 100.00% | 33.33% | 50.00% | 99.84% |
| **RF Tuned (Final)** ✅ | **99.52%** | **75.00%** | **100.00%** | **85.71%** | **99.84%** |

### Key Findings
- **PBT as % of Total Income** is the single strongest predictor of default (11.2% importance)
- **EPS** ranks #2 — a publicly visible, leading indicator that drops well before formal default
- Profitability margin ratios (PBT%, PAT%, Cash Profit%) collectively account for ~48% of model importance
- Company size (Total Assets, Net Worth) is **not** a significant predictor — large firms are equally at risk when margins deteriorate
- Optimal decision threshold: **0.985** (the model must be >98.5% confident before flagging a company)

---

## 📈 Part B — Stock Market Risk Analysis

### Problem Statement
Quantify the **risk-return profile** of five major Indian equities over an eight-year period (March 2016 – March 2024) to inform portfolio construction and allocation decisions.

### Stocks Analysed

| Stock | Sector | Price Range (INR) | Total Return |
|---|---|---|---|
| ITC Limited | FMCG | 156 – 493 | +97.7% |
| Bharti Airtel | Telecom | 261 – 1,236 | +291.1% |
| Tata Motors | Automotive | 65 – 1,035 | +153.9% |
| DLF Limited | Real Estate | 110 – 928 | **+659.6%** |
| Yes Bank | Banking | 11 – 397 | **-86.1%** |

### Risk-Return Summary

| Stock | Mean Weekly Return | Std Dev (Risk) | Sharpe-like Ratio | Assessment |
|---|---|---|---|---|
| ITC Limited | +0.2281% | 3.61% | 0.063 | 🟢 Defensive Anchor |
| Bharti Airtel | +0.4029% | 3.91% | **0.103** | 🟢 Core Holding |
| Tata Motors | +0.4088% | 6.20% | 0.066 | 🟡 Selective Exposure |
| DLF Limited | +0.6540% | 5.78% | **0.113** | 🟡 Tactical Growth |
| Yes Bank | -0.0475% | 9.11% | -0.005 | 🔴 **Avoid** |

### Portfolio Allocation Guidance

```
Bharti Airtel  ████████████████████████████████  30–35%  Core Holding
ITC Limited    ████████████████████████████      25–30%  Defensive Anchor
DLF Limited    ████████████████                  15–20%  Tactical Growth
Tata Motors    ████████████                      10–15%  Selective Exposure
Yes Bank       ██                                  < 5%  Speculative Only
```

---

## 🚀 Getting Started

### Prerequisites
- Python 3.8 or higher
- Jupyter Notebook or JupyterLab

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/FinRisk-ML.git
cd FinRisk-ML

# 2. (Recommended) Create a virtual environment
python -m venv venv
source venv/bin/activate        # macOS/Linux
venv\Scripts\activate           # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook
```

### Running the Notebooks

```bash
# Part A — Financial Default Prediction
jupyter notebook part_a/PartA_Financial_Default_Prediction.ipynb

# Part B — Stock Market Risk Analysis
jupyter notebook part_b/PartB_Stock_Market_Risk_Analysis.ipynb
```

> ⚠️ **Important:** Both notebooks expect the CSV data files to be in the `data/` folder.  
> The notebooks use relative paths — run them from the repository root or update the `pd.read_csv()` paths accordingly.

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---|---|---|
| `pandas` | ≥ 1.5 | Data manipulation and analysis |
| `numpy` | ≥ 1.23 | Numerical computations |
| `matplotlib` | ≥ 3.6 | Visualisation |
| `seaborn` | ≥ 0.12 | Statistical visualisation |
| `scikit-learn` | ≥ 1.2 | ML models, preprocessing, evaluation |

---

## 📊 Methodology Notes

### Why Recall over Accuracy?
In credit risk, a **false negative** (missing a real defaulter) is far costlier than a **false positive** (flagging a healthy company). Accuracy is misleading on imbalanced datasets — a model predicting "no default" for every company scores 94.5% accuracy while providing zero business value. **Recall and F1 Score** are the primary metrics.

### Why Random Forest over Logistic Regression?
Both tuned models achieve identical F1 (85.71%) and ROC-AUC (99.84%). The Random Forest is selected as the final model because:
1. **No VIF pre-processing required** — handles multicollinearity internally
2. **Captures non-linear relationships** between financial ratios
3. **No manual threshold calibration** needed at deployment
4. **Built-in feature importance** for model explainability

### Why Winsorisation over Outlier Removal?
Financial data naturally contains extreme values (micro-cap vs large-cap companies). Removing these would reduce the dataset and introduce survivorship bias. Winsorising at the 1st/99th percentile preserves the ordering and relative magnitude of values while preventing extreme observations from dominating model training.

---

## 📋 Business Report Presentations

Full business-oriented slide decks (no code) are available in the `reports/` folder:

- **`Financial_Default_Report.pptx`** — 19-slide deck covering the full default prediction pipeline, model results, feature importance, and risk response tiers (🔴 High / 🟡 Medium / 🟢 Low)
- **`Stock_Market_Risk_Analysis_Report.pptx`** — 15-slide deck covering price trends, return distributions, risk-return profiling, and portfolio allocation recommendations

---

## 🔑 Key Takeaways

1. **Profitability margins are the earliest warning signals.** PBT%, PAT%, and Cash Profit% collectively drive ~48% of the default prediction model. Watch these before balance sheet deterioration becomes visible.

2. **Company size does not equal safety.** Total Assets and Net Worth are not among the top predictors. A large firm with shrinking margins is just as at risk as a small one.

3. **Risk-adjusted return matters more than raw return.** DLF has the highest total return (+659.6%) but Bharti Airtel offers a superior risk-adjusted profile. Portfolio construction should target Sharpe-like efficiency, not just return maximisation.

4. **Yes Bank is a cautionary tale in both projects.** In Part B it represents catastrophic market risk (–86.1% total return). The governance-driven collapse mirrors the credit risk signals the Part A model is designed to catch early.

---

## 📄 License

This project is submitted as an academic assignment for the Great Learning Data Science programme. The datasets and business context are provided by Great Learning. All analysis, code, and documentation in this repository are original work.

---

## 🙋 Author

**[Your Name]**  
Data Science Programme — Great Learning  
April 2026

---

*For questions or feedback, open an issue or reach out via the repository's Discussions tab.*
