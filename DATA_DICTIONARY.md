# Data Dictionary

This document describes all variables in both datasets used across Part A and Part B.

---

## Part A Dataset — `Comp_Fin_Data.csv`

**4,256 companies × 51 columns**  
Source: Balance sheet financial metrics from company annual reports.  
All monetary values are in **Indian Rupees (₹ Crore)** unless stated otherwise.

### Identifier

| Column | Type | Description |
|---|---|---|
| `Num` | int | Row identifier — dropped before modelling |

### Target Variable

| Column | Type | Description |
|---|---|---|
| `Networth Next Year` | float | Net worth of the company in the following fiscal year. **Engineered into binary target:** `default = 1` if this value < 0, else `default = 0` |

### Income Statement Features

| Column | Type | Description | Missing % |
|---|---|---|---|
| `Total income` | float | Total revenue including all income sources | 5.4% |
| `Sales` | float | Revenue from core business operations (excludes financial income) | 7.2% |
| `Income from financial services` | float | Revenue from financial/lending activities | 26.1% |
| `Other income` | float | Non-operating income (interest, dividends, etc.) | 36.5% |
| `Total expenses` | float | Total cost of operations | 3.9% |
| `Profit after tax (PAT)` | float | Net profit after all deductions including tax | 3.6% |
| `PBDITA` | float | Profit Before Depreciation, Income Tax & Amortisation — equivalent to EBITDA | 3.6% |
| `PBT` | float | Profit Before Tax | 3.6% |
| `Cash profit` | float | Profit adjusted for non-cash items (PAT + Depreciation) | 3.6% |
| `Change in stock` | float | Change in inventory value between periods | 12.9% |

### Profitability Ratios

| Column | Type | Description | Missing % |
|---|---|---|---|
| `PBDITA as % of total income` | float | PBDITA / Total Income × 100 — operating profit margin | 1.9% |
| `PBT as % of total income` | float | PBT / Total Income × 100 — pre-tax profit margin | 1.9% |
| `PAT as % of total income` | float | PAT / Total Income × 100 — net profit margin | 1.9% |
| `Cash profit as % of total income` | float | Cash Profit / Total Income × 100 | 1.9% |
| `PAT as % of net worth` | float | PAT / Net Worth × 100 — Return on Equity proxy | 0% |

### Balance Sheet — Assets

| Column | Type | Description | Missing % |
|---|---|---|---|
| `Total assets` | float | Sum of all company assets (fixed + current + investments) | 0% |
| `Net fixed assets` | float | Fixed assets net of accumulated depreciation | 3.1% |
| `Investments` | float | Long-term financial investments | 40.3% |
| `Current assets` | float | Assets expected to be converted to cash within one year | 1.9% |

### Balance Sheet — Liabilities & Equity

| Column | Type | Description | Missing % |
|---|---|---|---|
| `Net worth` | float | Shareholders' equity = Total Assets − Total Liabilities | 0% |
| `Total capital` | float | Equity + long-term debt (capital base of the company) | 0.1% |
| `Shareholders funds` | float | Paid-up capital + reserves — equivalent to book equity | 0% |
| `Reserves and funds` | float | Accumulated retained earnings and statutory reserves | 2.3% |
| `Borrowings` | float | Total debt (short-term + long-term borrowings) | 10.1% |
| `Current liabilities & provisions` | float | Short-term obligations due within one year | 2.6% |
| `Deferred tax liability` | float | Tax obligation deferred to future periods | 32.1% |
| `Cumulative retained profits` | float | Profits retained in the business since inception | 1.1% |
| `Capital employed` | float | Net worth + long-term debt — the total capital base generating returns | 0% |
| `Total liabilities` | float | All obligations (current + non-current) | 0% |

### Leverage Ratios

| Column | Type | Description | Missing % |
|---|---|---|---|
| `TOL/TNW` | float | Total Outside Liabilities / Tangible Net Worth — key leverage indicator; higher = more risk | 0% |
| `Total term liabilities / tangible net worth` | float | Long-term debt relative to tangible equity | 0% |
| `Debt to equity ratio (times)` | float | Total debt / Shareholders' equity | 0% |
| `Contingent liabilities / Net worth (%)` | float | Off-balance-sheet risk relative to equity | 0% |
| `Contingent liabilities` | float | Potential future obligations (guarantees, lawsuits, etc.) | 32.9% |

### Liquidity Ratios

| Column | Type | Description | Missing % |
|---|---|---|---|
| `Quick ratio (times)` | float | (Current Assets − Inventory) / Current Liabilities — measures immediate liquidity | 2.5% |
| `Current ratio (times)` | float | Current Assets / Current Liabilities — short-term solvency | 2.5% |
| `Cash to current liabilities (times)` | float | Cash & equivalents / Current Liabilities | 2.5% |
| `Net working capital` | float | Current Assets − Current Liabilities | 0.9% |
| `Cash to average cost of sales per day` | float | Days of operating expenses covered by cash holdings | 2.3% |

### Efficiency / Turnover Ratios

| Column | Type | Description | Missing % |
|---|---|---|---|
| `Creditors turnover` | float | Cost of Sales / Trade Payables — measures payment speed to suppliers | 9.2% |
| `Debtors turnover` | float | Revenue / Trade Receivables — measures collection speed from customers | 9.0% |
| `Finished goods turnover` | float | Cost of Goods Sold / Finished Goods Inventory | 20.5% |
| `WIP turnover` | float | Cost of Production / Work-in-Progress Inventory | 17.9% |
| `Raw material turnover` | float | Raw Material Consumed / Raw Material Inventory | 10.1% |

### Market / Per Share Metrics

| Column | Type | Description | Missing % |
|---|---|---|---|
| `Shares outstanding` | float | Total number of shares issued by the company | 19.0% |
| `Equity face value` | float | Par value per share (typically ₹10 or ₹1) | 19.0% |
| `EPS` | float | Earnings Per Share = PAT / Shares Outstanding | 0% |
| `Adjusted EPS` | float | EPS adjusted for bonus issues and stock splits | 0% |
| `PE on BSE` | float | Price-to-Earnings ratio on Bombay Stock Exchange | **61.7%** |

---

## Part B Dataset — `Market_Risk_Data_coded.csv`

**418 rows × 6 columns**  
Weekly closing prices for five Indian equities.  
All prices are in **Indian Rupees (INR)**.

| Column | Type | Description |
|---|---|---|
| `Date` | str / datetime | Week-ending date in DD-MM-YYYY format. Parse with `pd.to_datetime(df['Date'], dayfirst=True)` |
| `ITC Limited` | int | Weekly closing price of ITC Limited (NSE: ITC) |
| `Bharti Airtel` | int | Weekly closing price of Bharti Airtel Ltd (NSE: BHARTIARTL) |
| `Tata Motors` | int | Weekly closing price of Tata Motors Ltd (NSE: TATAMOTORS) |
| `DLF Limited` | int | Weekly closing price of DLF Ltd (NSE: DLF) |
| `Yes Bank` | int | Weekly closing price of Yes Bank Ltd (NSE: YESBANK) |

### Derived Variables (Computed in Notebook)

| Variable | Formula | Description |
|---|---|---|
| `returns` | `df[stocks].pct_change() * 100` | Weekly percentage price change for each stock |
| `mean_return` | `returns.mean()` | Average weekly return (%) over the full period |
| `std_dev` | `returns.std()` | Standard deviation of weekly returns (%) — measure of risk |
| `sharpe_like` | `mean_return / std_dev` | Risk-adjusted return ratio (not annualised; for relative comparison only) |
| `rolling_vol` | `returns.rolling(52).std() * √52` | 52-week rolling annualised volatility |
| `normalised` | `(df[stocks] / df[stocks].iloc[0]) * 100` | Price index with base 100 at March 2016 |

---

## Notes on Data Quality

### Part A
- **PE on BSE** (61.7% missing) and **Investments** (40.3% missing) were dropped entirely in preprocessing due to excessive missingness.
- The remaining ~19 columns with partial missingness were addressed by removing affected rows after column-level dropping.
- Final clean dataset: **1,042 complete records** from the original 4,256 (24.5% retention).
- This retention rate reflects the compounding effect of multiple partially-missing columns. Future data collection should target the high-missingness fields.

### Part B
- No missing values. All 418 weekly observations are complete.
- Prices appear to be adjusted for corporate actions (bonus issues, splits) for Bharti Airtel and ITC — consistent with the smooth price series observed.
- **Yes Bank** prices do NOT appear adjusted for the 2020 rights issue at ₹12/share, which contributed to the sharp price step visible in the data around March 2020.
