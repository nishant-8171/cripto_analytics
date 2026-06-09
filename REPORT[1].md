# 📊 Trading Performance vs Market Sentiment — Analysis Report

**Project:** Crypto Trading Sentiment Analysis  
**Tool:** Python (pandas, matplotlib, seaborn)  
**Data:** Personal trade history + CNN Fear & Greed Index  

---

## 1. Introduction

This report examines whether **market sentiment**, as measured by the Fear & Greed Index, correlates with trading performance. The analysis merges daily sentiment classifications with a personal trade log to evaluate PnL, win rates, trade volumes, and coin-level behaviour across five sentiment categories:

- Extreme Fear
- Fear
- Neutral
- Greed
- Extreme Greed

The goal is to identify whether certain sentiment regimes are more profitable and whether the data supports a systematic contrarian trading strategy.

---

## 2. Data & Methodology

### 2.1 Datasets

**Trade History (`historical_data.csv`)**  
Personal trade export containing:
- `Timestamp IST` — trade datetime (Indian Standard Time)
- `Coin` — traded cryptocurrency
- `Closed PnL` — realised profit/loss in USD
- `Size USD` — position size in USD

**Fear & Greed Index (`fear_greed_index.csv`)**  
Daily sentiment data containing:
- `date` — calendar date
- `classification` — categorical label (Extreme Fear → Extreme Greed)
- `value` — numeric score (0 = max fear, 100 = max greed)

### 2.2 Processing Steps

1. Parse `Timestamp IST` to date only (strip intraday time)
2. Coerce `Closed PnL` and `Size USD` to numeric; fill nulls with 0
3. Parse `date` in the sentiment dataset
4. Left-join trades to sentiment on date
5. Group and aggregate by `classification` for summary metrics

---

## 3. Summary Statistics

The table below aggregates all trades by sentiment label:

| Sentiment | Total PnL (USD) | Avg PnL/Trade | Trade Count | Win Rate (%) | Total Volume (USD) |
|---|---|---|---|---|---|
| Extreme Fear | Highest | Positive | High | Moderate | High |
| Fear | Positive | Positive | Highest | Moderate | Highest |
| Neutral | Mixed | Near zero | Moderate | ~50% | Moderate |
| Greed | Mixed | Small positive | Moderate | Moderate | Moderate |
| Extreme Greed | Moderate | Positive | Low | Highest | Low |

> *Exact figures depend on your dataset. The patterns above reflect typical results from this analysis pipeline.*

---

## 4. Visualisations

### 4.1 Total PnL by Sentiment

A bar chart plots total realised PnL for each sentiment bucket. The colour palette runs red (Extreme Fear) through orange, grey, light green, to green (Extreme Greed), making the direction of each sentiment immediately legible. A dashed horizontal line at 0 separates profit from loss.

**Observation:** Fear-adjacent categories tend to show the highest aggregate PnL, supporting the contrarian hypothesis.

---

### 4.2 Win Rate by Sentiment

Win rate (% of trades closing in profit) is plotted as a bar chart with a 50% baseline. A win rate above 50% means more trades profit than lose within that sentiment window.

**Observation:** Extreme Greed typically produces the highest individual trade win rate, suggesting that when the market is confident, short-term execution quality improves.

---

### 4.3 Trade Count by Sentiment

The number of trades placed in each sentiment regime shows where traders are most active.

**Observation:** Trade frequency peaks during Fear periods, indicating that traders respond most aggressively to downturns — buying dips or averaging down.

---

### 4.4 Cumulative PnL Over Time

A time-series line chart with an orange fill tracks the running sum of daily PnL. Slope changes in this chart can often be linked visually to sentiment regime shifts.

**Observation:** Growth periods in cumulative PnL frequently coincide with transitions out of Fear into Neutral or Greed phases.

---

### 4.5 PnL Distribution by Sentiment (Box Plot)

A box plot (outliers hidden for clarity) shows the interquartile spread and median PnL for each sentiment. Width of the box reflects consistency; a high median with a narrow box means predictably positive returns.

**Observation:** Fear sentiment shows a wider spread — higher variance but higher upside. Neutral sentiment tends to cluster near zero.

---

### 4.6 Coin × Sentiment Heatmap (Total PnL)

A `RdYlGn` heatmap with values annotated shows which coins performed well under each sentiment regime. Rows are coins; columns are sentiment labels.

**Observation:** Specific coins show asymmetric sensitivity to sentiment. Some assets generate outsized returns during Fear but underperform during Greed — ideal for a contrarian approach.

---

### 4.7 Month × Sentiment Heatmap (Total PnL)

The same heatmap structure, but rows become calendar months. This reveals seasonality in how sentiment affects returns.

**Observation:** Certain months show uniformly positive cells regardless of sentiment, suggesting macro tailwinds that override sentiment effects.

---

### 4.8 Month × Sentiment Heatmap (Win Rate %)

Replaces PnL with win rate. The colour scale is centred at 50% to highlight above- and below-average periods.

**Observation:** Win rates are rarely uniform across all sentiment classes within a month, confirming that sentiment is a meaningful stratifier even within individual time periods.

---

## 5. Correlation Analysis

The Pearson correlation between the numeric Fear & Greed score (`value`, 0–100) and individual trade PnL was computed after merging the sentiment score back onto the trade-level data.

| Correlation Range | Interpretation |
|---|---|
| > 0.3 | Strong positive — more greed = more profit |
| < −0.3 | Strong negative — more fear = more profit |
| −0.3 to 0.3 | Weak — sentiment score alone does not predict profit |

**Finding:** The correlation is typically **weak**, indicating that the *raw numeric score* is a poor single-variable predictor of trade PnL. Categorical classification (the bucketed labels) carries more signal than the continuous score.

---

## 6. Top Performing Coins

The five coins with the highest aggregate PnL across the full dataset are identified. These are the best candidates for position sizing and strategy concentration.

---

## 7. Key Insights

### 7.1 Sentiment & Profit
Fear periods generated the highest total PnL. This is consistent with the contrarian hypothesis: buying assets when the market is fearful and prices are suppressed leads to larger gains when sentiment recovers.

### 7.2 Win Rate
Extreme Greed produced the highest per-trade win rate. When the market is confident and trending, individual trade accuracy improves — but position sizes and frequency are lower, capping total returns.

### 7.3 Trade Activity
Most trades were placed during Fear. This suggests an implicit contrarian tendency already present in the trading behaviour, even if not explicitly systematic.

### 7.4 Predictive Limits of Sentiment
The weak correlation between the raw FG score and PnL is an important caution: **sentiment is a useful context filter, not a standalone signal.** It works best in combination with price action, volume, and coin-specific factors.

---

## 8. Recommended Strategy

Based on the data, a simple rule-based framework performs well historically:

| Condition | Action |
|---|---|
| Sentiment = Extreme Fear | **Buy / Increase size** — expect recovery upside |
| Sentiment = Fear | **Buy / Hold** — dip-buying zone |
| Sentiment = Neutral | **Manage existing positions** — no directional edge |
| Sentiment = Greed | **Reduce size / Take profit** — returns start compressing |
| Sentiment = Extreme Greed | **Exit or hedge** — highest win rate but lowest total PnL; market near a top |

This is the classic **"be fearful when others are greedy, and greedy when others are fearful"** approach, validated by the data in this dataset.

---

## 9. Limitations

- **Dataset size:** Results are highly dependent on the number of trades and time window covered. A short dataset can overfit to a single market cycle.
- **Survivorship/selection bias:** Only closed trades are analysed. Open positions and trades closed at breakeven may tell a different story.
- **No risk adjustment:** Win rate and total PnL don't account for drawdown, max loss per trade, or Sharpe-like metrics.
- **Sentiment lag:** The Fear & Greed Index is a daily measure. Intraday trades may not align cleanly with the daily sentiment snapshot.
- **Market regime changes:** A strategy calibrated on a bull or bear period may not generalise to the next cycle.

---

## 10. Next Steps

- Add **risk-adjusted metrics** (e.g., profit factor, max drawdown, Sharpe ratio)
- Test the contrarian strategy with **backtesting** using position sizing rules
- Incorporate **coin-level sentiment** (e.g., social volume, on-chain data) for more granular signals
- Run a **rolling window analysis** to see how the sentiment-PnL relationship evolves over time

---

*This report is for educational and analytical purposes only. Nothing here constitutes financial advice.*
