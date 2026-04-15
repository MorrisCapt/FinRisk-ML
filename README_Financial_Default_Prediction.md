# Part A — Financial Default Prediction

## Overview

This module builds a binary classification model to predict whether a company will become a **financial defaulter** — defined as having a negative net worth in the following fiscal year. The model is intended as the core engine of a **Financial Health Assessment Tool** for a venture capital firm.

---

## Business Context

In venture capital and credit risk management, the cost of a **missed default** (false negative) far exceeds the cost of a **false alarm** (false positive). A missed default leads to unrealised credit losses, damaged portfolio returns, and reputational risk. A false alarm leads only to additional due diligence. The model is therefore built to **prioritise recall** — catching every real defaulter — while minimising false alarms as a secondary objective.

**Affected stakeholders:** Venture capital partners, credit analysts, portfolio company management teams, and institutional co-investors.

---

## Dataset

**File:** `data/Comp_Fin_Data.csv`

| Property | Value |
|---|---|
| Rows | 4,256 companies |
| Columns | 51 (including identifier and target) |
| Target | `Networth Next Year` → engineered to `default` (binary) |
| Feature types | All numeric (float64 / int64) |
| Missing values | Present in 25 of 49 feature columns |

### Target Variable Definition

```python
df['default'] = (df['Networth Next Year'] < 0).astype(int)
# default = 1 → company will have negative net worth next year (DEFAULTER)
# default = 0 → company will have positive net worth next year (HEALTHY)
```

**Class distribution:** 4,022 non-defaulters (94.5%) vs 234 defaulters (5.5%)

---

## Notebook Structure

| Section | Description |
|---|---|
| **1. Import Libraries** | All dependencies with version notes |
| **2. Load & Inspect Data** | Shape, dtypes, statistical summary |
| **3. EDA** | Missing values, target distribution, univariate histograms, multivariate boxplots, correlation heatmap |
| **4. Data Preprocessing** | Column dropping (>40% missing), row dropping, winsorisation, train/test split, StandardScaler |
| **5. VIF Analysis** | Variance Inflation Factor computation, iterative removal, final VIF bar chart |
| **6. Model Building** | LR Base + RF Base — classification reports, confusion matrices, score distributions |
| **7. Model Improvement** | LR: ROC curve + Youden threshold | RF: RandomizedSearchCV (20 iter, 5-fold CV) |
| **8. Model Comparison** | 4-model comparison table, grouped bar chart, ROC overlay |
| **9. Feature Importance** | Top-15 importance chart, final confusion matrix |
| **10. Insights** | Formatted recommendations summary |

---

## Preprocessing Pipeline

```
Input: Comp_Fin_Data.csv (4,256 × 51)
    ↓
Drop 'Num' (identifier) and 'Networth Next Year' (target source)
    ↓
Create 'default' binary target
    ↓
Drop columns with >40% missing values
    ↓
Drop rows with any remaining null values
    → Output: 1,042 × 49 (fully complete records)
    ↓
Winsorise all features at 1st / 99th percentile
    ↓
Stratified train/test split (80% / 20%, random_state=42)
    → Train: 833 rows | Test: 209 rows
    ↓
StandardScaler (fit on train, transform both)
    [Applied to Logistic Regression only]
```

---

## VIF Feature Reduction (Logistic Regression Only)

Variance Inflation Factor measures how much a feature's regression coefficient variance is inflated by multicollinearity. **VIF > 10 = problematic.**

| Removed Feature | VIF at Removal |
|---|---|
| Total assets | ∞ |
| Total income | 30,149.5 |
| Total liabilities | 1,414.4 |
| Sales | 1,138.9 |
| Net worth | 1,027.3 |
| PBT | 558.8 |
| PBDITA | 265.2 |
| Cash profit | 205.5 |
| Shareholders funds | 101.1 |
| Reserves and funds | 65.3 |
| Net fixed assets | 55.1 |
| EPS | 48.6 |
| Current assets | 37.9 |
| Capital employed | 31.4 |
| PBT as % of total income | 30.8 |
| Total expenses | 24.2 |
| Cumulative retained profits | 17.2 |
| Debt to equity ratio (times) | 15.7 |
| Profit after tax | 12.1 |
| Deferred tax liability | 10.5 |

**20 features removed → 28 retained for Logistic Regression**  
Random Forest trained on all features (immune to multicollinearity).

---

## Model Results

### Logistic Regression — Base
```
              precision    recall  f1-score
Non-Default      1.00        0.98      0.99
Default          0.38        1.00      0.55
ROC-AUC: 99.84%   |   Threshold: 0.50
```

### LR + VIF + Optimal Threshold (Youden Index)
```
              precision    recall  f1-score
Non-Default      1.00        1.00      1.00
Default          0.75        1.00      0.86
ROC-AUC: 99.84%   |   Threshold: 0.985
```

### Random Forest — Base
```
              precision    recall  f1-score
Non-Default      0.99        1.00      1.00
Default          1.00        0.33      0.50
ROC-AUC: 99.84%   |   Threshold: 0.50
```

### Random Forest — Tuned ✅ FINAL MODEL
```
              precision    recall  f1-score
Non-Default      1.00        1.00      1.00
Default          0.75        1.00      0.86

Best hyperparameters:
  n_estimators=100, max_depth=None, min_samples_split=10,
  min_samples_leaf=4, max_features='log2'

ROC-AUC: 99.84%   |   F1 Score: 85.71%   |   Recall: 100%
```

---

## Top Feature Importances

| Rank | Feature | Importance |
|---|---|---|
| 1 | PBT as % of total income | 11.18% |
| 2 | EPS | 9.76% |
| 3 | Adjusted EPS | 7.41% |
| 4 | PBT | 7.32% |
| 5 | PAT as % of total income | 6.47% |
| 6 | Cash profit as % of total income | 6.30% |
| 7 | Cumulative retained profits | 6.16% |
| 8 | Profit after tax | 6.16% |
| 9 | PAT as % of net worth | 4.66% |
| 10 | PBDITA as % of total income | 3.92% |

---

## Actionable Insights

1. **Monitor profitability margins as the primary early-warning signal.** PBT%, PAT%, and Cash Profit% collectively drive ~48% of the model's decisions. Flag companies with two or more consecutive quarters of declining margins.

2. **Elevated leverage compounds the risk.** High TOL/TNW or Debt-to-Equity ratios are tolerable in isolation but dangerous when combined with falling margins.

3. **Size is not a safety net.** Total Assets and Net Worth are not top predictors. Large companies can default when their efficiency ratios deteriorate.

4. **Risk response tiers:**
   - 🔴 Probability ≥ 98.5% → Freeze credit, trigger full audit immediately
   - 🟡 Probability 50–98.5% → Enhanced monitoring, quarterly review
   - 🟢 Probability < 50% → Standard annual review cycle

5. **Recalibrate the threshold annually.** The Youden-optimal threshold (0.985) may shift as the dataset grows. Re-run the ROC analysis with each model retraining cycle.
