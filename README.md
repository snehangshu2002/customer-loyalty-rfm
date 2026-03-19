# RFM Customer Loyalty Analysis

A comprehensive RFM (Recency, Frequency, Monetary) analysis project for customer segmentation and loyalty scoring using Python.

## Overview

This project analyzes customer transaction data to segment customers based on their purchasing behavior. RFM analysis helps identify:
- **Recency (R)**: How recently a customer made a purchase
- **Frequency (F)**: How often a customer makes purchases
- **Monetary (M)**: How much money a customer spends

## Methodology

### RFM Scoring

Customers are scored on a 1-4 scale using quartile-based ranking:

| Metric | Description | Scoring Logic |
|--------|-------------|---------------|
| **Recency** | Days since last purchase | Lower = Higher score (1=recent, 4=inactive) |
| **Frequency** | Number of orders | Higher = Higher score |
| **Monetary** | Total spend | Higher = Higher score |

### Loyalty Score Calculation

```
LoyaltyScore = R_Rank + F_Rank + M_Rank
```

### Loyalty Badge Tiers

| Badge | Score Range | Description |
|-------|-------------|-------------|
| **Platinum** | Lowest scores (3-5) | Most valuable customers |
| **Gold** | Low scores (6-7) | Loyal customers |
| **Silver** | Medium scores (8-9) | Regular customers |
| **Bronze** | High scores (10-12) | At-risk customers |

## Project Structure

```
.
├── data/
│   ├── raw/                    # Raw transaction data
│   └── processed/              # Processed RFM scores
├── notebooks/
│   └── rfm_analysis.ipynb      # Main analysis notebook
├── outputs/
│   ├── charts/                 # Generated visualizations
│   └── reports/                # Summary reports
├── main.py                     # Standalone RFM script
├── pyproject.toml              # Python dependencies
└── README.md                   # This file
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

### Jupyter Notebook

Run the main analysis notebook:

```bash
jupyter notebook notebooks/rfm_analysis.ipynb
```

### Python Script

Run the standalone analysis:

```bash
python main.py
```

## Key Insights from Analysis

### Customer Distribution
- **Gold** is the dominant segment (~289 customers) — potential to convert to Platinum
- **Platinum** customers are the most valuable and recent buyers
- **Bronze** customers (122) are priority for win-back campaigns

### Correlation Findings
| Finding | Correlation | Action |
|---------|-------------|--------|
| Frequent buyers spend more | 0.54 | Target frequent buyers for upsell |
| Recency barely affects spend | -0.14 | Don't ignore lapsed high-spenders |
| Recent = slightly more frequent | -0.29 | Re-engage lapsed to boost frequency |

### Distribution Patterns
- **Recency**: Most customers purchased recently (0-100 days), with a long tail of inactive customers
- **Frequency**: Bell-shaped distribution peaking around 10-15 orders
- **Monetary**: Heavily right-skewed; majority spend $0-$2,500, few high-value customers up to $25,000

## Data Requirements

Input CSV should contain:
- `Customer ID` / `CustomerID`: Unique customer identifier
- `Order ID` / `OrderID`: Transaction identifier
- `Order Date` / `OrderDate`: Transaction date
- `Sales`: Transaction amount

## Outputs

The analysis generates:
- `segmented_data.csv` — Customer-level RFM scores and loyalty badges
- Distribution charts (Recency, Frequency, Monetary)
- Loyalty badge visualizations
- Correlation heatmaps
- Treemap of customer segments

## Technologies Used

- **Python 3.x**
- **pandas** — Data manipulation
- **numpy** — Numerical operations
- **matplotlib** & **seaborn** — Visualization
- **squarify** — Treemap visualization
- **jupyter** — Interactive analysis

## License

This project is for educational and analytical purposes.
