# Algorithmic Trading in Python
### RSI + Momentum Strategy with Efficient Frontier Analysis

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

---

## Overview

This project implements a quantitative trading strategy that combines **RSI (Relative Strength Index)** and **6-month price momentum** to generate buy and sell signals across a universe of large-cap US technology stocks. The strategy is then evaluated against an **Efficient Frontier** analysis to assess its positioning relative to mean-variance optimal portfolios.

---

## Strategy Logic

**Buy signal** — triggered when both conditions are met simultaneously:
- 6-month momentum is above its historical mean (positive trend)
- RSI is below its historical mean (stock is relatively oversold)

**Sell signal** — triggered when:
- 6-month momentum turns strongly negative (below negative mean)
- RSI rises above 60 (overbought territory)

**Position sizing** — equal weight distributed across all qualifying tickers on each day.

---

## Universe & Periods

| Parameter | Value |
|-----------|-------|
| Tickers | AAPL, NVDA, MSFT, TSLA, NFLX, META |
| Benchmark | SPY |
| Full period | May 2012 – Jan 2023 |
| Training period | May 2012 – Dec 2019 |
| Test period | Jan 2020 – Jan 2023 |
| Momentum lookback | 126 trading days (~6 months) |
| RSI lookback | 14 days |
| Transaction cost | 0.5% per trade |

---

## Notebook Structure

| Section | Description |
|---------|-------------|
| **Data Import** | Downloads OHLCV data via `yfinance` / `bt`, defines train/test splits |
| **Momentum & RSI** | Calculates 126-day momentum and 14-day RSI for all tickers |
| **Visualisation** | Interactive price + momentum + RSI charts per ticker |
| **Signal Generation** | Derives buy/sell conditions and builds daily target weight matrices |
| **Filter Statistics** | Descriptive stats (mean, median, std) for RSI and momentum in both periods |
| **Strategy Backtest** | Runs bt backtests for the active strategy and equal-weight benchmark |
| **Strategy Comparison** | Side-by-side performance: active vs benchmark, train vs test |
| **Parameter Heatmap** | Grid search over 36 momentum × RSI lookback combinations (CAGR & Sharpe) |
| **Efficient Frontier** | Plots the EF, Max Sharpe, HRP, and the MR strategy point together |
| **Results Summary** | Final expected return and risk for the combined strategy |

---

## Key Results

The parameter sensitivity heatmap tests 6 momentum windows (32–1008 days) × 6 RSI windows (1–35 days) and visualises CAGR and Sharpe across all 36 combinations, allowing identification of robust parameter regions rather than overfitting to a single setting.

The Efficient Frontier section compares:
- **Max Sharpe portfolio** (red star)
- **HRP portfolio** (orange diamond)
- **Momentum + RSI strategy** (blue pentagon)

against 10,000 randomly simulated portfolios using training-period data.

---

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/algo-trading-rsi-momentum.git
cd algo-trading-rsi-momentum
pip install -r requirements.txt
jupyter notebook Algorithmic_Trading_in_Python.ipynb
```

> **Note on PyPortfolioOpt:** If `pip install PyPortfolioOpt` fails in your environment (common on Google Colab due to NumPy/SciPy version conflicts), run `pip install PyPortfolioOpt --upgrade` in a separate cell first. Sections 1–8 of the notebook run without it.

---

## Dependencies

| Library | Purpose |
|---------|---------|
| `bt` | Strategy backtesting engine |
| `yfinance` | Market data download |
| `pandas` / `numpy` | Data manipulation |
| `matplotlib` / `seaborn` | Visualisation |
| `PyPortfolioOpt` | Efficient frontier, HRP optimisation |

Full version-pinned list in `requirements.txt`.

---

## Project Structure

```
algo-trading-rsi-momentum/
├── Algorithmic_Trading_in_Python.ipynb   ← Main notebook
├── requirements.txt                       ← Pinned dependencies
├── README.md
└── .gitignore
```

---

## Reference

Supporting calculations and data tables available in the companion Google Sheet:
[View Sheet](https://docs.google.com/spreadsheets/d/1UTc6IPuWEgLhRVwyranhKZWKuQd0B2qe/edit?usp=sharing)

---

## License

MIT — free to use, modify, and distribute with attribution.
