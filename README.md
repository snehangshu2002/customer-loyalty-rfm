# RFM Customer Loyalty Analysis
 
A comprehensive RFM (Recency, Frequency, Monetary) analysis project for customer segmentation and loyalty scoring using Python.
 
## Overview
 
This project analyzes customer transaction data to segment customers based on their purchasing behavior. RFM analysis helps identify:
- **Recency (R)**: How recently a customer made a purchase
- **Frequency (F)**: How often a customer makes purchases
- **Monetary (M)**: How much money a customer spends
 
## Dataset
 
The analysis is based on the **Superstore** dataset containing 9,994 transaction rows across 29 columns including Order ID, Order Date, Customer ID, Sales, Quantity, Discount, Profit, and various categorical fields.
 
## Methodology
 
### Data Preparation
 
1. Select columns needed for RFM: `Customer ID`, `Order ID`, `Order Date`, `Sales`
2. Convert `OrderDate` to datetime
3. Remove 2 duplicate rows
4. Set the **analytical date** = last purchase date + 1 day (`2017-12-31`)
 
### RFM Scoring
 
Customers are scored on a 1–4 scale using quartile-based ranking:
 
| Metric | Description | Scoring Logic |
|--------|-------------|---------------|
| **Recency** | Days since last purchase | Lower days = Rank 1 (most recent); higher = Rank 4 (inactive) |
| **Frequency** | Count of orders | Higher = Rank 1; lower = Rank 4 |
| **Monetary** | Total spend | Higher = Rank 1; lower = Rank 4 |
 
**Quantile thresholds (Q1 / Q2 / Q3):**
| Metric | Q1 | Q2 | Q3 |
|--------|----|----|----|
| Recency | 31 days | 76 days | 184 days |
| Frequency | 8 orders | 12 orders | 16 orders |
| Monetary | $1,146 | $2,256 | $3,785 |
 
### Loyalty Score Calculation
 
```
LoyaltyScore = R_Rank + F_Rank + M_Rank
Range: 3 (best) → 12 (worst)
```
 
### Loyalty Badge Tiers
 
Badges are assigned via `pd.qcut` on the combined loyalty score:
 
| Badge | Description |
|-------|-------------|
| **Platinum** | Lowest scores — most valuable customers |
| **Gold** | Low-medium scores — loyal customers |
| **Silver** | Medium scores — regular customers |
| **Bronze** | High scores — at-risk customers |
 

## Project Structure

```
.
├── data/
│   ├── RFM analysis output data/   # RFM analysis outputs
│   │   └── segmented_data.csv      # Customer segments with RFM scores
│   ├── superstore_clean.csv        # Cleaned transaction data
│   └── superstore_raw.csv          # Original raw data
├── notebooks/
│   ├── 01_data_preparation.ipynb   # Data cleaning notebook
│   └── rfm_analysis.ipynb          # Main RFM analysis notebook
├── .gitignore                      # Git ignore rules
├── CLAUDE.md                       # Claude Code configuration
├── LICENSE                         # Project license
├── pyproject.toml                  # Python dependencies (uv)
├── README.md                       # This file
└── uv.lock                         # Locked dependency versions
```

## Installation
 
This project uses `uv` for Python dependency management:
 
```bash
# Install dependencies
uv sync
 
# Activate virtual environment (Windows)
.venv\Scripts\activate
 
# Activate virtual environment (Unix/Mac)
source .venv/bin/activate
```
 
## Usage

### Jupyter Notebooks

Run the notebooks in order:

```bash
# 1. Data preparation and cleaning
jupyter notebook notebooks/01_data_preparation.ipynb

# 2. Main RFM analysis and segmentation
jupyter notebook notebooks/rfm_analysis.ipynb
```
 
## Key Insights from Analysis
 
### Customer Distribution
 
| Badge | Count | Share |
|-------|-------|-------|
| **Gold** | ~289 | ~37% |
| **Platinum** | ~128 | ~16% |
| **Silver** | ~94 | ~12% |
| **Bronze** | ~122 | ~16% |
 
- Over **60% of customers** fall in Gold or Platinum — healthy loyalty base
- **Gold is the dominant segment** (~289 customers) — strong opportunity to convert to Platinum
- **Bronze customers (122)** are the priority for win-back campaigns
 
### Recency / Frequency / Monetary Distributions
 
| Metric | Pattern |
|--------|---------|
| **Recency** | Most customers purchased recently (0–100 days), with a long tail to ~1,200 days |
| **Frequency** | Bell-shaped distribution peaking around 10–15 orders |
| **Monetary** | Heavily right-skewed; majority spend $0–$2,500; a few high-value customers up to $25,000 |
 
### Monetary Value by Badge
 
- Gold tier contains the **highest single spender (~$25K)** — review for Platinum upgrade
- Bronze customers are **consistently low spenders** with little variation
- Outliers in Platinum suggest **VIP customers** worth special attention
 
### Correlation Findings
 
| Finding | Correlation | Action |
|---------|-------------|--------|
| Frequent buyers spend more | **0.54** | Target frequent buyers for upsell |
| Recency barely affects spend | **-0.14** | Don't ignore lapsed high-spenders |
| Recent = slightly more frequent | **-0.29** | Re-engage lapsed customers to boost frequency |
 
## Data Requirements
 
Input CSV should contain:
- `Customer ID` / `CustomerID`: Unique customer identifier
- `Order ID` / `OrderID`: Transaction identifier
- `Order Date` / `OrderDate`: Transaction date
- `Sales`: Transaction amount
 
## Outputs
 
The analysis generates:
- `data/RFM analysis output data/segmented_data.csv` — Customer-level RFM scores and loyalty badges
- Distribution charts (Recency, Frequency, Monetary)
- Loyalty badge count bar chart
- Monetary value boxplot by badge
- Recency vs. Monetary scatter plot coloured by badge
- Correlation heatmap (R / F / M)
- Treemap of customer segments by badge
 
## Technologies Used
 
- **Python 3.x**
- **pandas** — Data manipulation
- **numpy** — Numerical operations
- **matplotlib** & **seaborn** — Visualization
- **squarify** — Treemap visualization
- **jupyter** — Interactive analysis
 
## License
 
This project is for educational and analytical purposes.