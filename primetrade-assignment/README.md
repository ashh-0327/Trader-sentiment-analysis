# Trader Performance vs Market Sentiment Analysis

## Primetrade.ai – Data Science / Analytics Intern Assignment

### Overview

This project analyzes the relationship between Bitcoin market sentiment (Fear & Greed Index) and trader performance on the Hyperliquid trading platform.

The objective is to understand how market sentiment influences trader behavior, profitability, leverage usage, and trading patterns, and to derive actionable trading strategies based on the observed insights.

The analysis combines historical trading records with daily market sentiment data to answer business-oriented questions using statistical analysis, trader segmentation, and unsupervised learning.

---

## Problem Statement

Can market sentiment explain differences in trader performance and behavior?

This project investigates:

- Do traders perform differently during Fear and Greed markets?
- Does trading behavior change with sentiment?
- Which types of traders consistently outperform others?
- Can these insights be converted into practical trading strategies?

---

## Dataset

Two datasets were provided.

### 1. Bitcoin Fear & Greed Index

Contains daily market sentiment labels.

Columns include:

- Date
- Classification (Fear / Greed)

---

### 2. Hyperliquid Historical Trading Data

Contains historical trade-level information including:

- Account
- Symbol
- Side (Buy/Sell)
- Execution Price
- Position Size
- Leverage
- Closed PnL
- Fees
- Timestamp
- Event Type

---

## Project Workflow

### Data Preparation

- Loaded and inspected both datasets
- Removed duplicates
- Checked missing values
- Converted timestamps into daily dates
- Merged trading activity with daily market sentiment

---

### Feature Engineering

Daily account-level metrics were created, including:

- Daily Profit & Loss
- Number of Trades
- Win Rate
- Average Trade Size
- Average Leverage
- Long / Short Ratio
- Profit Factor
- Sharpe-like Ratio
- Drawdown
- Maximum Drawdown

---

### Statistical Analysis

Market performance was compared across different sentiment conditions using:

- Median comparisons
- Distribution analysis
- Mann–Whitney U statistical test

Metrics compared include:

- Daily PnL
- Win Rate
- Drawdown
- Trade Frequency
- Trade Size
- Long / Short Ratio

---

### Trader Segmentation

Traders were categorized into three behavioral groups:

- Frequent vs Infrequent Traders
- Large-size vs Small-size Traders
- Consistent vs Inconsistent Traders

Performance differences across these groups were analyzed under varying market sentiment.

---

### Trader Archetypes

K-Means clustering was used to identify behavioral trader archetypes based on:

- Trading Frequency
- Trade Size
- Win Rate
- Long/Short Ratio
- Profit Variability
- Profit Factor
- Sharpe-like Ratio

Principal Component Analysis (PCA) was used to visualize the resulting clusters.

---

## Key Insights

- Market sentiment influences both trader behavior and trading outcomes.
- Fear and Greed periods exhibit noticeable differences in profitability and trading activity.
- Frequent traders generally outperform infrequent traders over time.
- Consistent traders experience lower variability in returns.
- Behavioral clustering reveals distinct trader archetypes with different risk-return characteristics.

---

## Strategy Recommendations

Based on the analysis, the following rules of thumb are proposed:

### Strategy 1

Increase trading activity only for traders with historically consistent performance during favorable market sentiment.

### Strategy 2

Reduce trading exposure for inconsistent or infrequent traders during volatile market conditions to minimize drawdowns.

These recommendations are supported by statistical comparisons and trader segmentation.

---

## Visualizations

The project includes:

- Fear vs Greed Performance Comparison
- Trading Behavior Analysis
- Trader Segmentation Charts
- Trader Archetype (PCA) Visualization
- Cluster Heatmaps

All generated outputs are available in the `outputs/` directory.

---

## Project Structure

```
Trader_sentiment_analysis/
│
├── analysis.ipynb
├── README.md
├── REPORT.md
├── requirements.txt
│
├── data/
│   ├── historical_data.csv
│   └── fear_greed.csv
│
└── outputs/
    ├── charts/
    └── tables/
```

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- SciPy
- Scikit-learn
- Jupyter Notebook

---

## Installation
Install dependencies

```bash
pip install -r requirements.txt
```

---

## How to Run

Open

```
analysis.ipynb
```

Run all notebook cells from top to bottom.

The notebook automatically:

- loads the datasets
- preprocesses the data
- performs feature engineering
- generates statistical analysis
- creates visualizations
- exports all tables and charts into the `outputs/` folder

---

## Output

Generated outputs include:

### Charts

- performance_and_behavior.png
- trader_archetypes.png
- segments.png

### Tables

- fear_vs_greed_stats.csv
- account_segments.csv
- trader_archetypes.csv
- strategy2_recommendations.csv

---

## Repository Highlights

- End-to-end reproducible workflow
- Daily sentiment alignment with trading activity
- Statistical hypothesis testing
- Trader segmentation
- Behavioral clustering
- Actionable strategy recommendations
- Clean and modular notebook structure