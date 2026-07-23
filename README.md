# Trader Performance vs. Market Sentiment

Analysis of Hyperliquid trading data against the Bitcoin Fear & Greed Index, submitted as the data science take-home task for Primetrade.ai.

## Overview

This project merges 29,979 trades from 8 Hyperliquid accounts (Jan 2024 – May 2025) with the daily Bitcoin Fear & Greed Index to test whether market sentiment is associated with trader profitability, position sizing, and behavior.

**Key finding:** Traders in this dataset perform *better* during Fear/Extreme Fear than during Greed/Extreme Greed — the opposite of a naive "ride the greed" assumption. Average PnL per trade during Extreme Fear ($565) is nearly 4x the average during Extreme Greed ($147). However, this pattern is not uniform across individual accounts — see Section 6 of the report.

## Repo Contents

```
├── README.md                          # this file
├── analysis.py                        # full analysis pipeline (data → charts)
├── Trader_Sentiment_Analysis.pdf      # written report with findings
├── chart1_pnl_winrate.png             # avg PnL & win rate by sentiment
├── chart2_position_size.png           # avg position size by sentiment
├── chart3_correlation.png             # daily PnL vs. sentiment index, scatter + trend
└── chart4_peraccount_performance.png  # per-account PnL by sentiment (heatmap)
```

## Data

Not included in this repo (see original task email for source links):
- `historical_data.csv` — Hyperliquid trade-level data (Account, Coin, Execution Price, Size Tokens, Size USD, Side, Timestamp, Start Position, Direction, Closed PnL, etc.)
- `fear_greed_index.csv` — daily Bitcoin Fear & Greed Index (date, value, classification)

Place both files in the same directory as `analysis.py` before running (or update `DATA_DIR` at the top of the script).

## How to Run

```bash
pip install pandas numpy matplotlib
python analysis.py
```

Outputs all four charts to `./output/` and prints the underlying summary tables to the console.

## Method Summary

1. **Merge**: trades matched to Fear & Greed classification by calendar date (99.98% match rate).
2. **Performance by regime**: restricted to trades with non-zero closed PnL (realized closes only, n ≈ 13,245) to isolate actual trade outcomes from opens/partial fills.
3. **Position sizing**: average trade size (USD) and buy/sell counts by sentiment regime, across all trades.
4. **Correlation**: daily average closed PnL vs. raw Fear & Greed index value (0–100), Pearson r = -0.16 (weak).
5. **Per-account variation**: avg PnL by account × sentiment for the 6 most active accounts, to check whether the aggregate pattern holds at the individual-trader level.

## Known Limitations

- The `leverage` field mentioned in the original task brief is **not present** in the raw data file; position sizing (Size USD) was used as the closest available proxy.
- Sample is 8 accounts only — a small, non-random slice of Hyperliquid users. Findings should not be generalized to the broader market.
- Correlation ≠ causation: sentiment may be a lagging co-indicator of the same volatility/liquidity conditions that drive PnL, rather than a direct cause. This has not been tested with a control for volatility or market trend.
- Per-account results (Section 6 / chart4) vary widely in sample size per cell — cells with small n should be read with proportionally less confidence.


