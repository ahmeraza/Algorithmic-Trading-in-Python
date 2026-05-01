# Changelog

All notable changes to this project are documented here.

---

## [1.2.0] — 2026-04-30

### Added
- **Objective section** at the start of the notebook — strategy rationale, asset selection justification with table, and methodology overview
- **Financial insight callouts** after every major result (12 new markdown cells) — explains what each output means in plain financial terms
- **Key Outcomes section** at the end — summary of findings and an actionable decision table covering 6 scenarios (deploy, overweight, exit, blend, pause, recalibrate)
- SPY overlay on both training and test comparison charts for direct market context

### Improved
- All code cells now have brief inline comments explaining *what* each block does and *why*
- Section headers restructured (Section 1–6) for clearer navigation
- Weight construction cell now explains the index alignment requirement
- Transaction cost cell clarifies the 0.5% assumption and its real-world basis

---

## [1.1.0] — 2026-04-30

### Fixed
- Removed duplicate `benchmark` data fetch (fetched same data as `data`, never used)
- Replaced deprecated `DataFrame._append()` in heatmap loop with `pd.concat` pattern
- Moved `rsi_calc2` function definition outside the heatmap loop (was redefined 36 times)
- Replaced informal "suffering from bugs" comment with a proper technical explanation

### Improved
- Cleaned up `pip install` cell — removed duplicate `--quiet` flag, split PyPortfolioOpt onto its own line with install note
- Added `plt.tight_layout()` after all backtest plots to prevent label clipping
- Added SPY as benchmark overlay on training and test comparison plots

---

## [1.0.0] — 2026-04-30

### Initial release
- RSI + Momentum strategy backtested across 6 US tech stocks (2012–2023)
- Train/test split: May 2012–Dec 2019 training, Jan 2020–Jan 2023 test
- Equal-weight benchmark comparison
- Parameter sensitivity heatmap (36 combinations)
- Efficient Frontier analysis with Max Sharpe and HRP portfolios
- README, requirements.txt, and .gitignore included
