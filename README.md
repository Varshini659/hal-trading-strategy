# HAL NSE Algorithmic Trading Strategy
### ML-driven directional signal + backtest on Hindustan Aeronautics Ltd (HAL.NS)

---

## Project Overview
Built an end-to-end quantitative trading pipeline for a defence-sector equity
(HAL, NSE) using XGBoost to predict next-day price direction, then backtested
the resulting signal against a buy-and-hold benchmark over a held-out
14-month test window (Nov 2024 - Dec 2025).

---

## Results - Test Period (Nov 2024 - Dec 2025, 288 trading days)

| Metric                  | ML Strategy | Buy & Hold |
|-------------------------|-------------|------------|
| Total Return            | **25.6%**   | 2.9%       |
| CAGR                    | **22.0%**   | 2.6%       |
| Sharpe Ratio            | **0.92**    | 0.24       |
| Max Drawdown            | -29.6%      | -          |
| Win Rate (in-market)    | 56.6%       | -          |
| Days in Market          | 122 / 288 (42%) | -      |
| ROC-AUC                 | 0.5644      | -          |

**ML strategy outperformed buy-and-hold by 8.8× on total returns
and delivered 3.8× better risk-adjusted performance (Sharpe Ratio).**

---

## Key Findings
- **Long-term trend (SMA_50) and volume anomalies (Volume_Ratio) were the
  two most predictive features** - RSI, despite being a popular retail indicator,
  had the lowest importance score, suggesting short-term oscillators add
  limited signal for HAL over this period.
- The model demonstrated **stronger precision on "Down" days (63%) than
  "Up" days (48%)** - a systematic asymmetry that warrants further investigation
  and potential asymmetric position sizing in future iterations.
- Spending only **42% of days in the market** (vs 100% for buy-and-hold)
  while delivering 8.8× the return suggests the model's primary value is
  in **avoiding prolonged drawdown periods**, not just capturing upside.
- Max Drawdown of **-29.6%** remains a significant risk. A stop-loss overlay
  or volatility-scaled position sizing would be the logical next step
  to improve the risk profile.

---

## Tech Stack
- **Data:** yfinance (Yahoo Finance API), 5 years daily OHLCV
- **Features:** SMA-20/50, RSI-14, MACD/Signal/Histogram, 20-day Volatility,
  Volume Ratio, Daily Return
- **Model:** XGBoost Classifier (n_estimators=200, max_depth=4, lr=0.05)
- **Validation:** Chronological walk-forward split (80/20) - no data leakage
- **Metrics:** ROC-AUC, Sharpe Ratio, CAGR, Max Drawdown, Win Rate

---

## Project Structure
hal-trading-strategy/

├── hal_trading.ipynb      ← main notebook (data → features → model → backtest)

├── requirements.txt

└── README.md

## Limitations & Next Steps
- Single stock backtest - expanding to a basket (BEL, BDL, MSTC) would
  test generalisability across the defence sector
- No transaction costs or slippage modelled - real execution would erode
  returns by an estimated 0.1-0.3% per trade
- Hyperparameter tuning via time-series cross-validation (TimeSeriesSplit)
  could improve AUC further
- Stop-loss or volatility-scaled position sizing to reduce the -29.6%
  max drawdown


