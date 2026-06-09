# 📈 Crypto Trading Analysis: Fear & Greed Sentiment Study

A data analysis project exploring the relationship between **market sentiment** (Fear & Greed Index) and **trading performance** (PnL, win rate, trade volume) across multiple cryptocurrencies.

---

## 📌 Project Overview

This notebook merges personal trade history with the CNN Fear & Greed Index to answer one key question:

> *Does market sentiment predict trading profitability?*

It segments performance by sentiment classification (Extreme Fear → Extreme Greed), surfaces patterns across coins and time periods, and closes with a data-backed strategy suggestion.

---

## 📂 Repository Structure

```
├── trading.ipynb          # Main analysis notebook
├── REPORT.md              # Full written report with findings & insights
├── README.md              # This file
└── data/
    ├── historical_data.csv    # Personal trade history (Timestamp, Coin, PnL, Size USD, etc.)
    └── fear_greed_index.csv   # Daily Fear & Greed classification + score
```

> **Note:** The `data/` files are not included in this repository. Place your own CSVs in a `data/` folder and update the file paths in the notebook accordingly.

---

## 🔧 Setup & Usage

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn
```

### Running the Notebook

1. Clone the repo:
   ```bash
   git clone https://github.com/YOUR_USERNAME/trading-sentiment-analysis.git
   cd trading-sentiment-analysis
   ```

2. Add your data files to the `data/` folder.

3. Update the file paths in Cell 2 of the notebook:
   ```python
   trd_df = pd.read_csv("data/historical_data.csv")
   smt_df = pd.read_csv("data/fear_greed_index.csv")
   ```

4. Launch Jupyter and run all cells:
   ```bash
   jupyter notebook trading.ipynb
   ```

---

## 📊 Data Sources

| Dataset | Description | Columns Used |
|---|---|---|
| `historical_data.csv` | Personal trade log (e.g. from Hyperliquid/Binance export) | `Timestamp IST`, `Coin`, `Closed PnL`, `Size USD` |
| `fear_greed_index.csv` | Daily Fear & Greed Index | `date`, `classification`, `value` |

---

## 📉 Analysis Sections

1. **Data Loading & Cleaning** — Parse dates, coerce numerics, handle nulls
2. **Sentiment Merge** — Join trade history with daily sentiment labels
3. **Summary Statistics** — Total PnL, avg PnL, win rate, volume by sentiment
4. **Visualizations**
   - Bar charts: PnL, win rate, trade count by sentiment
   - Cumulative PnL over time
   - Box plot: PnL distribution per sentiment
   - Heatmaps: Coin × Sentiment and Month × Sentiment (PnL & Win Rate)
5. **Correlation Analysis** — Fear & Greed score vs trade PnL
6. **Key Insights & Strategy Suggestion**

---

## 💡 Key Findings

- **Fear periods** generated the highest total PnL — buying the dip works
- **Extreme Greed** had the highest win rate per trade
- **Most trades** were placed during Fear — traders are active when markets are down
- **Weak correlation** between raw FG score and PnL: sentiment alone doesn't predict profit
- Classic **contrarian strategy** (buy Fear, sell Greed) is supported by the data

For the full breakdown, see [`REPORT.md`](REPORT.md).

---

## 🛠️ Tech Stack

- **Python 3.x**
- `pandas` — data wrangling
- `numpy` — numerical operations
- `matplotlib` — base plotting
- `seaborn` — statistical visualizations

---

## 📄 License

This project is for personal/educational use. No financial advice is implied or intended.
