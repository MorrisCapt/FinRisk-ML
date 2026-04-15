# Part B — Stock Market Risk Analysis

## Overview

This module analyses weekly closing prices for five major Indian equities over an eight-year period (March 2016 – March 2024) to quantify their **risk-return profiles** and deliver portfolio allocation recommendations.

---

## Business Context

Equity investing requires balancing expected return against the risk incurred to achieve it. Selecting stocks purely on price appreciation ignores volatility — and volatility represents the real cost of holding an asset. This analysis applies standard quantitative finance techniques to produce a **risk-return scatter plot**, enabling fund managers and retail investors to make evidence-based allocation decisions.

**Stocks covered:** Five Indian equities spanning five distinct sectors — FMCG, Telecom, Automotive, Real Estate, and Banking.

**Constraints:** Analysis is backward-looking (historical data only). No transaction costs, dividends, taxes, or short-selling are modelled.

---

## Dataset

**File:** `data/Market_Risk_Data_coded.csv`

| Property | Value |
|---|---|
| Rows | 418 weekly observations |
| Columns | 6 (Date + 5 stock prices) |
| Date range | 28 March 2016 – 25 March 2024 |
| Frequency | Weekly (Monday close) |
| Missing values | None |

### Stocks

| Column | Company | Sector | Price Range (INR) |
|---|---|---|---|
| `ITC Limited` | ITC Ltd | FMCG / Tobacco | 156 – 493 |
| `Bharti Airtel` | Bharti Airtel Ltd | Telecom | 261 – 1,236 |
| `Tata Motors` | Tata Motors Ltd | Automotive | 65 – 1,035 |
| `DLF Limited` | DLF Ltd | Real Estate | 110 – 928 |
| `Yes Bank` | Yes Bank Ltd | Banking | 11 – 397 |

---

## Notebook Structure

| Section | Description |
|---|---|
| **1. Import Libraries** | All dependencies |
| **2. Load & Inspect Data** | Shape, date range, statistical summary |
| **3. Stock Price Graph Analysis** | Individual 5-panel price series with min/max annotations, normalised price index (base=100), total return table |
| **4. Returns Calculation** | Weekly returns formula, bar chart time series, return distribution histograms |
| **5. Mean & Std Dev** | Full statistics table (mean, std, min, max, skewness, kurtosis, Sharpe-like ratio), mean bar chart, risk bar chart |
| **6. Mean vs Std Dev Plot** | Risk-return scatter with quadrant labels, rolling 52-week annualised volatility, returns correlation heatmap |
| **7. Insights** | Formatted portfolio recommendations |

---

## Returns Methodology

### Weekly Return Formula

```
Return(t) = [Price(t) - Price(t-1)] / Price(t-1) × 100  (%)
```

- 418 price points → **417 weekly return observations** per stock
- Computed using `pandas.DataFrame.pct_change() * 100`

### Annualised Volatility (Rolling)

```
Annual Vol = Rolling_52_week_std × √52
```

This provides a time-varying view of risk, capturing volatility regime changes (e.g., the COVID crash in March 2020).

### Sharpe-like Ratio

```
Sharpe-like = Mean Weekly Return / Std Dev of Weekly Returns
```

Note: A true Sharpe Ratio uses excess return over the risk-free rate. This simplified version uses raw mean return and is intended for **relative comparison** between stocks only, not as an absolute risk-adjusted measure.

---

## Results Summary

### Price Performance (March 2016 – March 2024)

| Stock | Start Price (INR) | End Price (INR) | Total Return | CAGR (approx.) |
|---|---|---|---|---|
| ITC Limited | 217 | 428 | **+97.7%** | ~8.8% |
| Bharti Airtel | 316 | 1,237 | **+291.1%** | ~18.5% |
| Tata Motors | 386 | 985 | **+153.9%** | ~12.4% |
| DLF Limited | 114 | 866 | **+659.6%** | ~28.7% |
| Yes Bank | 173 | 24 | **−86.1%** | −24.6% |

### Risk-Return Statistics (Weekly Returns)

| Stock | Mean Return (%) | Std Dev (%) | Sharpe-like | Profile |
|---|---|---|---|---|
| ITC Limited | +0.2281 | 3.6127 | 0.0631 | 🟢 Defensive |
| Bharti Airtel | +0.4029 | 3.9073 | 0.1031 | 🟢 Core holding |
| Tata Motors | +0.4088 | 6.1976 | 0.0660 | 🟡 Cyclical |
| DLF Limited | +0.6540 | 5.7796 | 0.1132 | 🟡 Growth |
| Yes Bank | −0.0475 | 9.1095 | −0.0052 | 🔴 Avoid |

### Portfolio Allocation (Suggested)

```
Bharti Airtel  ████████████████████████████████  30–35%  Core Holding
ITC Limited    ████████████████████████████      25–30%  Defensive Anchor
DLF Limited    ████████████████                  15–20%  Tactical Growth
Tata Motors    ████████████                      10–15%  Selective Exposure
Yes Bank       ██                                  < 5%  Speculative Only
```

---

## Key Findings

### 1. Yes Bank — A Structural Warning
Yes Bank is the only stock with a **negative mean return** combined with the **highest risk** in the group. Its collapse in 2020 (governance crisis + RBI-mandated rescue) reduced share value by over 97% from peak to trough. Four years later, no recovery to pre-crisis levels occurred. This is a textbook case of **idiosyncratic risk** overwhelming sector-level dynamics — and mirrors the type of corporate distress the Part A model is designed to flag early.

### 2. Bharti Airtel — Best Risk-Adjusted Return
Bharti Airtel's prolonged trough (2018–2020, driven by intense competition following Reliance Jio's market entry) was followed by a sustained, accelerating recovery. Its **Sharpe-like ratio of 0.103** is the second-highest in the group. The ongoing 5G rollout and improving Average Revenue Per User (ARPU) support continued momentum.

### 3. DLF — Highest Total Return, Cyclical Risk
DLF's +659.6% total return is exceptional, but almost entirely concentrated in 2023–2024 — a late-cycle real estate surge driven by low inventory and post-pandemic demand. Real estate is highly sensitive to interest rate cycles; any tightening may compress returns going forward.

### 4. Diversification Is Partial, Not Complete
Return correlations between most pairs range from **0.20 to 0.46** — meaningful co-movement, but not extreme. This provides partial (not complete) diversification benefit. The five stocks span five sectors, which naturally reduces concentration risk. A true minimum-variance portfolio would weight positions inversely to their volatility contribution.

### 5. Volatility Clustering Is Universal
All five stocks exhibit **volatility clustering** — periods of calm punctuated by turbulent episodes. The March 2020 COVID crash is visible as a simultaneous spike across all stocks. Post-2021, most stocks returned to lower volatility regimes, suggesting improving market conditions.

---

## Actionable Recommendations

| Recommendation | Rationale |
|---|---|
| **Make Bharti Airtel the core position** | Best Sharpe-like ratio (0.103); strong fundamental recovery |
| **Use ITC as a portfolio anchor** | Lowest volatility (3.61%); resilient during market downturns |
| **Hold DLF tactically, not permanently** | Highest return but cyclical; trim if interest rates rise |
| **Size Tata Motors smaller than Airtel** | Same return profile at 59% higher risk |
| **Exclude Yes Bank from institutional portfolios** | Negative return + highest risk = no justifiable allocation |
| **Apply risk-parity sizing** | Allocate inversely to Std Dev to equalise risk contributions |
| **Review allocations annually** | Sectoral conditions evolve; DLF and Tata Motors especially sensitive to macro cycles |
