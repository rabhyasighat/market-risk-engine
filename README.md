# Market Risk Engine

A comprehensive market risk measurement framework built in Python, implementing
three industry-standard VaR methodologies, regulatory backtesting, and 
historical stress testing across a multi-asset portfolio.


---

## Overview

This project replicates the core quantitative risk tools used by risk desks
at investment banks and asset managers. It covers the full workflow from raw
market data to regulatory reporting metrics.

**Portfolio:** 6-asset multi-class portfolio — US equities, S&P 500 index,
EUR/USD FX, and Gold futures — with £10 million notional value.

**Data:** 6 years of daily OHLCV data (2018–2024) sourced via `yfinance`.

---

## Methods Implemented

### Value at Risk (VaR) & CVaR
| Method | Description |
|---|---|
| Historical VaR | Empirical percentile of past return distribution |
| Parametric VaR (Normal) | Variance-covariance with normal distribution assumption |
| Parametric VaR (t-dist) | Fitted Student's t — captures fat tails in financial returns |
| Monte Carlo VaR | 10,000 correlated simulations via Cholesky decomposition |

### Backtesting
- Rolling 1-year VaR backtest with exceedance flagging
- **Kupiec Proportion of Failures (POF) test** — formal statistical model validation
- **Basel III traffic light framework** — Green / Amber / Red classification

### Stress Testing
- 5 historical crisis replays: GFC 2008, COVID 2020, Dot-com bust, Russia-Ukraine, Rate hike cycle
- 5 hypothetical shock scenarios: equity crash, flight to safety, USD strength, stagflation, recovery
- Cumulative P&L, worst single day, annualised crisis volatility, max drawdown per scenario

---

## Results

### VaR Comparison (99% confidence, 1-day horizon)

| Method | VaR (%) | VaR (£) |
|---|---|---|
| Historical |  -3.210% | £320,992 |
| Parametric (Normal) | -2.625% | £262,502 |
| Parametric (t-dist) | -3.156% | £315,583 |
| Monte Carlo | -2.668% | £266,843 |


### Backtesting
- Kupiec POF test: **FAIL** (p < 0.05)
- Basel traffic light: **GREEN**

---

## Project Structure
```
market-risk-engine/
│
├── market_risk_engine.ipynb   # Main notebook — full analysis
├── README.md                  # This file
└── requirements.txt           # Python dependencies
```

---

## Setup & Usage

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/market-risk-engine.git
cd market-risk-engine
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the notebook
Open `market_risk_engine.ipynb` in Jupyter and run all cells top to bottom.
All data is fetched automatically via `yfinance` — no manual data download needed.

---

## Requirements
```
yfinance>=0.2.0
pandas>=1.5.0
numpy>=1.23.0
scipy>=1.9.0
matplotlib>=3.6.0
plotly>=5.10.0
statsmodels>=0.13.0
jupyterlab>=3.5.0
```

---

## Key Concepts

**VaR (Value at Risk):** The minimum loss expected on a bad day at a given
confidence level. A 99% 1-day VaR of £200,000 means losses will exceed
£200,000 on approximately 1 in 100 trading days.

**CVaR (Expected Shortfall):** The average loss on days that breach VaR.
More informative than VaR because it captures the severity of tail losses,
not just the threshold. Preferred by regulators under Basel III.

**Kupiec Test:** A likelihood ratio test that formally evaluates whether
the observed breach rate is statistically consistent with the model's
stated confidence level.

**Cholesky Decomposition:** A matrix factorisation technique used in Monte
Carlo simulation to generate correlated random asset returns that respect
the empirical covariance structure of the portfolio.

---

## Limitations & Extensions

**Known limitations:**
- Assumes returns are i.i.d. — ignores volatility clustering (GARCH effects)
- Historical VaR is sensitive to the lookback window chosen
- Monte Carlo assumes normally distributed shocks — fat tails not fully captured

**Natural extensions:**
- GARCH(1,1) volatility forecasting for time-varying VaR
- Filtered Historical Simulation combining GARCH with empirical residuals
- Expected Shortfall backtesting (Acerbi-Szekely test)
- Multi-period VaR scaling using the square root of time rule

---

## Background

This project was built as part of a portfolio targeting market risk and
quantitative risk roles in London. The methodology follows industry practice
as described in:

- Basel Committee on Banking Supervision — *Minimum Capital Requirements 
  for Market Risk* (FRTB, 2019)
- Jorion, P. — *Value at Risk: The New Benchmark for Managing Financial Risk*
- Hull, J. — *Options, Futures and Other Derivatives*

---

## Author

**Rabhya Sighat**  
MSc Risk Management & Financial Engineering — Imperial Business School  
[LinkedIn](https://www.linkedin.com/in/rabhya-sharabjit-sighat/) | 
[GitHub](https://github.com/rabhyasighat)
```

---
