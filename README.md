# RFM Customer Loyalty Analysis
 
RFM (Recency, Frequency, Monetary) analysis on Superstore transaction data. Segments customers by purchase behaviour and assigns loyalty badges.
 
## What it does
 
Each customer gets scored 1–4 on three dimensions:
 
- **Recency** — days since their last order (lower = better score)
- **Frequency** — number of orders placed (higher = better score)
- **Monetary** — total spend (higher = better score)
 
The three ranks are summed into a loyalty score (3–12), then split into four tiers via `pd.qcut`:
 
| Badge | Score range | Who they are |
|-------|-------------|--------------|
| Platinum | 3–5 | Highest-value, most active customers |
| Gold | 6–7 | Loyal, regular buyers |
| Silver | 8–9 | Occasional customers |
| Bronze | 10–12 | Lapsed or low-value — candidates for win-back |
 
## Dataset
 
Superstore sales data — 9,994 rows, 29 columns. Only four columns are used for RFM: `Customer ID`, `Order ID`, `Order Date`, `Sales`.
 
Two duplicate rows were dropped. The analytical reference date is `2017-12-31` (last purchase date + 1 day).
 
## Quantile thresholds
 
| Metric | Q1 | Q2 | Q3 |
|--------|----|----|----|
| Recency | 31 days | 76 days | 184 days |
| Frequency | 8 orders | 12 orders | 16 orders |
| Monetary | $1,146 | $2,256 | $3,785 |
 

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
├── LICENSE                         # Project license
├── pyproject.toml                  # Python dependencies (uv)
├── README.md                       # This file
└── uv.lock                         # Locked dependency versions
```

## Installation
 
Uses `uv` for dependency management:
 
```bash
uv sync
 
# Windows
.venv\Scripts\activate
 
# Unix/Mac
source .venv/bin/activate
```
 
## Usage
 
```bash
# Notebook
jupyter notebook notebooks/rfm_analysis.ipynb
 
```
 
## Results
 
### Customer distribution
 
| Badge | Count |
|-------|-------|
| Gold | ~289 |
| Bronze | ~122 |
| Platinum | ~128 |
| Silver | ~94 |
 
Gold is the largest group. The notebook flags ~289 Gold customers as upgrade candidates if recency improves. Bronze (122 customers) is the win-back target.
 
### Distribution patterns
 
- **Recency**: Most customers bought within the last 100 days. A long tail extends to ~1,200 days.
- **Frequency**: Bell-shaped, peaks around 10–15 orders.
- **Monetary**: Right-skewed. Most customers spend under $2,500; a few reach $25K.
 
### Monetary by badge
 
The highest single spender (~$25K) sits in Gold, not Platinum — worth reviewing for a tier upgrade. Bronze spend is tightly clustered at the low end with few outliers.
 
### Correlations
 
| Finding | r | So what |
|---------|---|---------|
| Frequency → Monetary | 0.54 | Frequent buyers spend more — good upsell targets |
| Recency → Monetary | -0.14 | Lapsed customers aren't necessarily low-value |
| Recency → Frequency | -0.29 | Re-engaging lapsed customers tends to lift order count |
 
## Outputs
 
- `segmented_data.csv` — one row per customer with RFM scores and badge
- Distribution histograms (Recency, Frequency, Monetary)
- Badge count bar chart
- Monetary boxplot by badge
- Recency vs. Monetary scatter plot
- Correlation heatmap
- Treemap of badge distribution
 
## Input format
 
Any CSV with these columns (exact names or the variants shown):
 
| Column | Variants accepted |
|--------|------------------|
| Customer ID | `CustomerID` |
| Order ID | `OrderID` |
| Order Date | `OrderDate` |
| Sales | — |
 
## Dependencies
 
- `pandas` — data wrangling
- `numpy` — numerical ops
- `matplotlib`, `seaborn` — charts
- `squarify` — treemap
- `jupyter` — notebook
 
## License
 
Educational use.
 