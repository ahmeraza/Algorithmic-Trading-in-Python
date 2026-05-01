# 📈 Algorithmic Trading in Python
### RSI + Momentum Strategy with Efficient Frontier Analysis

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![bt](https://img.shields.io/badge/bt-0.2.10-informational)
![PyPortfolioOpt](https://img.shields.io/badge/PyPortfolioOpt-1.5.5-blueviolet)

---

A **quantitative trading research project** that develops, backtests, and evaluates a systematic RSI + Momentum strategy across US large-cap technology stocks. The strategy is validated out-of-sample and benchmarked against both a passive equal-weight portfolio and SPY, then positioned on the Efficient Frontier alongside Max Sharpe and Hierarchical Risk Parity (HRP) portfolios.

> 📊 Companion data & calculations: [Google Sheets](https://docs.google.com/spreadsheets/d/1UTc6IPuWEgLhRVwyranhKZWKuQd0B2qe/edit?usp=sharing)

---

## 📌 Table of Contents

- [Strategy Overview](#-strategy-overview)
- [Asset Universe & Rationale](#-asset-universe--rationale)
- [Methodology](#-methodology)
- [Notebook Structure](#-notebook-structure)
- [Key Results](#-key-results)
- [Installation](#-installation)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Dependencies](#-dependencies)
- [Contributing](#-contributing)
- [Changelog](#-changelog)
- [License](#-license)

---

## 🧠 Strategy Overview

The strategy combines two classical technical indicators to generate systematic entry and exit signals.

### Buy Signal
Triggered when **both** conditions are met simultaneously:

| Condition | Meaning |
|-----------|---------|
| 6-month momentum ≥ its historical mean | Stock is in a confirmed uptrend |
| RSI < its historical mean | Stock is relatively oversold within that uptrend |

> This targets the **"pullback within an uptrend"** pattern — a historically high-probability entry for momentum-driven large-caps.

### Sell Signal
Triggered when:

| Condition | Meaning |
|-----------|---------|
| 6-month momentum < negative mean | Trend has reversed |
| RSI > 60 | Stock has become overbought |

### Position Sizing
- **Equal weight** distributed across all qualifying tickers each day
- **Cash** is held on days when no ticker meets both conditions — the strategy does not force allocation

---

## 📦 Asset Universe & Rationale

| Ticker | Company | Why Included |
|--------|---------|--------------|
| `AAPL` | Apple | Largest market cap; high liquidity, persistent trend behaviour |
| `NVDA` | NVIDIA | High momentum characteristics; driven by semiconductor super-cycles |
| `MSFT` | Microsoft | Defensive growth; lower correlation with pure cyclicals |
| `TSLA` | Tesla | High volatility and momentum; strong RSI signal testing ground |
| `NFLX` | Netflix | Strong growth trends with periodic deep corrections |
| `META` | Meta | High-beta tech; highly sensitive to sentiment shifts captured by RSI |

These six names share **momentum-driven, large-cap** characteristics with sufficient price history from 2012. Their divergent risk profiles (defensive MSFT vs. volatile TSLA) allow the strategy to demonstrate selective signal generation rather than indiscriminate buying.

---

## 🔬 Methodology

```
Data (2012–2023)
     │
     ├── Training Period (2012–2019) ──► Fit RSI + Momentum thresholds
     │                                   ► Generate buy/sell signals
     │                                   ► Build daily weight matrix
     │                                   ► Run bt backtest
     │                                   ► Parameter grid search (heatmap)
     │                                   ► Efficient Frontier analysis
     │
     └── Test Period (2020–2023) ────► Apply same signal logic out-of-sample
                                       ► Run bt backtest
                                       ► Compare vs benchmark & SPY
```

### Parameters

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| Momentum lookback | 126 trading days | ~6 months; captures medium-term trend |
| RSI lookback | 14 days | Standard Wilder RSI; balances sensitivity and noise |
| Sell RSI level | 60 | Conservative; triggers before overbought extreme |
| Transaction cost | 0.5% per trade | Accounts for bid-ask spread and brokerage commissions |
| Training period | May 2012 – Dec 2019 | Pre-COVID bull market for initial calibration |
| Test period | Jan 2020 – Jan 2023 | Includes crash, rally, and rate-hike correction |

### Portfolio Benchmarks

| Portfolio | Method | Purpose |
|-----------|--------|---------|
| Equal-Weight | Hold all 6 stocks equally throughout | Baseline passive strategy |
| SPY | Buy-and-hold S&P 500 ETF | Broad market benchmark |
| Max Sharpe | Mean-variance optimisation | Theoretically optimal risk-adjusted portfolio |
| HRP | Hierarchical Risk Parity | Risk-balanced diversification without return forecasts |

---

## 📓 Notebook Structure

| Section | Description |
|---------|-------------|
| **1 — Objective** | Strategy rationale, asset selection justification, methodology overview |
| **2 — Data Import** | Downloads price data via `bt` / `yfinance`; defines train/test splits |
| **3 — Momentum & RSI** | 126-day momentum and 14-day RSI calculated for all tickers |
| **4 — Visualisation** | Two-panel chart: price + momentum (top), RSI (bottom), SPY overlay |
| **5 — Signal Generation** | Buy/sell conditions with financial explanation; daily weight matrices |
| **6 — Filter Statistics** | RSI and momentum descriptive stats with regime interpretation |
| **7 — Strategy Backtests** | Active strategy and equal-weight benchmark across both periods |
| **8 — Strategy Comparison** | Side-by-side vs benchmark + SPY overlay; training and test periods |
| **9 — Parameter Heatmap** | 36-combination grid search (CAGR & Sharpe) to assess parameter robustness |
| **10 — Efficient Frontier** | EF, Max Sharpe, HRP, and MR strategy plotted with financial insights |
| **11 — Results Summary** | Formatted metrics table: CAGR, Sharpe, Max Drawdown, Volatility |
| **12 — Key Outcomes** | Actionable decision framework based on the full analysis |

---

## 📊 Key Results

### Strategy vs Benchmarks

> Run the notebook to populate with live results. The table below shows the metrics tracked across both periods.

| Metric | MR Strategy | Equal-Weight | SPY |
|--------|------------|-------------|-----|
| CAGR (%) | — | — | — |
| Sharpe Ratio | — | — | — |
| Max Drawdown (%) | — | — | — |
| Volatility (%) | — | — | — |

*Training: May 2012 – Dec 2019 · Test: Jan 2020 – Jan 2023*

### Parameter Sensitivity

The grid search tests **6 momentum windows** (32–1,008 days) × **6 RSI windows** (1–35 days) = **36 combinations**, visualised as CAGR and Sharpe heatmaps.

A robust strategy shows a **cluster** of strong cells, not a single isolated peak. The default parameters (126-day momentum, 14-day RSI) sit within the consistently green region — confirming they are not arbitrarily tuned to the training period.

### Efficient Frontier Positioning

Three portfolios are plotted against the frontier (10,000 random simulations):

| Portfolio | Marker | Characteristic |
|-----------|--------|---------------|
| Max Sharpe | 🔴 Red star | Highest risk-adjusted return; concentrates in 2-3 assets |
| HRP | 🟠 Orange diamond | Balanced risk allocation; better drawdown control |
| Momentum + RSI | 🔵 Blue pentagon | Rule-based; no return forecasts required |

The gap between the strategy and the frontier quantifies the efficiency cost of using signals vs mathematical optimisation — a known and accepted trade-off for live implementability.

---

## ⚙️ Installation

### Prerequisites

- Python 3.10 or higher
- pip
- Jupyter Notebook or JupyterLab

### Clone & Install

```bash
git clone https://github.com/YOUR_USERNAME/algo-trading-rsi-momentum.git
cd algo-trading-rsi-momentum
pip install -r requirements.txt
```

### Launch Notebook

```bash
jupyter notebook Algorithmic_Trading_in_Python.ipynb
```

> **Note on PyPortfolioOpt:** If installation fails (common on Google Colab due to NumPy/SciPy version conflicts), run this in a separate cell first:
> ```bash
> pip install PyPortfolioOpt --upgrade
> ```
> Sections 1–8 run without it. Only Section 10 (Efficient Frontier) requires it.

---

## 🚀 Usage

### Running the Full Analysis

1. Open `Algorithmic_Trading_in_Python.ipynb` in Jupyter
2. Run **Cell 1** to install dependencies (first time only)
3. Run all cells via `Kernel → Restart & Run All`
4. Review outputs in Sections 7–12

### Customising the Strategy

| What to change | Where in notebook | Example |
|---------------|-------------------|---------|
| Ticker universe | Section 2, `tickers` list | Add `'amzn'` or remove `'nflx'` |
| Date range | Section 2, `start_date` / `end_date` | Extend to `'2024-01-01'` |
| Momentum window | Section 3, `shift(126)` | Change to `63` for 3-month momentum |
| RSI window | Section 3, `rsi_calc(price, n=14)` | Change to `21` for smoother RSI |
| Sell RSI threshold | Section 5, `rsi_df > 60` | Change to `70` for later exits |
| Transaction cost | Section 6, `transaction_cost = 0.005` | Change to `0.001` for institutional costs |

### Changing the Chart Ticker

In Section 4, change the ticker in `plot_momentum_rsi()` to inspect any asset in the universe:

```python
plot_momentum_rsi(
    ticker='nvda',   # change to: 'aapl', 'msft', 'tsla', 'nflx', 'meta'
    rsi_df=rsi_train,
    ...
)
```

---

## 📁 Project Structure

```
algo-trading-rsi-momentum/
│
├── Algorithmic_Trading_in_Python.ipynb   ← Main research notebook (12 sections)
│
├── requirements.txt                       ← Pinned Python dependencies
├── CHANGELOG.md                           ← Version history
├── README.md                              ← This file
└── .gitignore                             ← Excludes checkpoints, caches, OS files
```

---

## 📚 Dependencies

| Library | Version | Purpose |
|---------|---------|---------|
| `bt` | 0.2.10 | Backtesting engine — portfolio construction, rebalancing, performance stats |
| `yfinance` | 0.2.40 | Market data download (used internally by `bt.get()`) |
| `pandas` | 2.2.2 | Data manipulation and time-series alignment |
| `numpy` | 1.26.4 | Numerical computations (matrix operations for EF) |
| `matplotlib` | 3.9.0 | All charts and visualisations |
| `seaborn` | 0.13.2 | Parameter sensitivity heatmaps |
| `PyPortfolioOpt` | 1.5.5 | Efficient Frontier, Max Sharpe optimisation, HRP |

Install all dependencies:

```bash
pip install -r requirements.txt
```

---

## 🤝 Contributing

Contributions, suggestions, and extensions are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Make your changes with clear inline comments
4. Test by running the full notebook (`Kernel → Restart & Run All`)
5. Submit a pull request with a brief description of what changed and why

### Ideas for Extension

- Add additional tickers (e.g. `AMZN`, `GOOGL`) or extend the date range to 2024–2025
- Implement a **volatility filter** (e.g. VIX threshold) to suppress signals during high-uncertainty regimes
- Replace the single train/test split with **rolling walk-forward validation**
- Add a **Sortino ratio** and **Calmar ratio** to the results summary
- Export the final results to a formatted **HTML or PDF report**

---

## 📋 Changelog

See [CHANGELOG.md](CHANGELOG.md) for the full version history.

**Latest — v1.2.0**
- Added Objective section with asset rationale and methodology overview
- Added 12 financial insight callouts throughout the notebook
- Added Key Outcomes section with actionable decision framework
- Added inline comments to all code cells

---

## 📄 License

MIT — free to use, modify, and distribute with attribution.

---

*This project is for educational and research purposes only. Nothing in this repository constitutes financial advice. Past performance does not guarantee future results.*
