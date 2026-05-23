# Trader Performance vs Market Sentiment
### Data Science / Analytics Intern — Round-0 Assignment | Primetrade.ai

---

## Objective

Analyze how **Bitcoin Market Sentiment (Fear/Greed)** relates to **trader behavior and performance** on Hyperliquid. The goal is to uncover patterns that could inform smarter trading strategies.

---

## Project Structure

```
├── analysis.ipynb        # Main notebook with full analysis
├── fear_greed_index.csv  # Bitcoin Fear/Greed dataset
├── historical_data.csv   # Hyperliquid trader data
├── README.md             # This file
```

---

## Datasets

### 1. Bitcoin Market Sentiment (Fear/Greed)
- **File:** `fear_greed_index.csv`
- **Rows:** 2,644 | **Columns:** 4
- **Columns:** `timestamp`, `value`, `classification`, `date`
- **Sentiment labels:** Extreme Fear, Fear, Neutral, Greed, Extreme Greed

### 2. Hyperliquid Trader Data
- **File:** `historical_data.csv`
- **Rows:** 211,224 | **Columns:** 16
- **Key columns:** `Account`, `Coin`, `Execution Price`, `Size USD`, `Side`, `Timestamp IST`, `Closed PnL`, `Fee`

---

## Setup & How to Run

### Requirements

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn
```

Or install from requirements file:

```bash
pip install -r requirements.txt
```

### Requirements.txt contents

```
pandas
numpy
matplotlib
seaborn
```

### Running the Notebook

1. Clone or download this repository
2. Place both CSV files in the same folder as `analysis.ipynb`
3. Open the notebook:

```bash
jupyter notebook analysis.ipynb
```

4. Run all cells from top to bottom:
   - **Kernel → Restart & Run All**

---

## Methodology

### Part A — Data Preparation

- Loaded both datasets and documented shape, missing values, and duplicates
- **No missing values or duplicates** found in either dataset
- Converted timestamps:
  - `Timestamp IST` (format: `DD-MM-YYYY HH:MM`) → date using `pd.to_datetime` with `format='%d-%m-%Y %H:%M'`
  - `timestamp` (Unix epoch in seconds) → date using `pd.to_datetime` with `unit='s'`
- Aggregated trader data to **daily level** per account
- Computed key metrics:
  - Daily PnL per trader
  - Win rate (winning trades / total trades)
  - Average trade size (USD)
  - Number of trades per day
  - Long/Short ratio (Buy count / Sell count)
  - Drawdown proxy (cumulative PnL − rolling max)
- Merged both datasets on `date` column for Part B analysis

---

### Part B — Analysis

**Q1: Does performance differ between Fear vs Greed days?**
- Grouped daily metrics by sentiment label
- Compared avg PnL, win rate, and drawdown across all 5 sentiment categories

**Q2: Do traders change behavior based on sentiment?**
- Analyzed trade frequency, position size, and long/short ratio by sentiment
- Found traders are more long-biased during Greed and reduce position sizes during Fear

**Q3: Trader Segments**
- **Segment 1 — Frequent vs Infrequent:** Split at median avg daily trade count
- **Segment 2 — Consistent Winners vs Inconsistent:** Split at median avg daily PnL

**Q4: 3 Key Insights**
- PnL distribution comparison: Fear vs Greed days
- Win rate by trader segment across sentiments
- Avg PnL by winner segment across sentiments

---

## Key Insights

| Insight | Finding |
|---------|---------|
| **Fear days ≠ bad days** | Avg PnL is actually positive on Fear days but drawdown is highest, meaning high volatility |
| **Extreme Greed = highest win rate** | Traders win more individual trades during Extreme Greed (38.6% win rate vs 32.9% on Extreme Fear) |
| **Greed brings worst drawdowns** | Avg drawdown on Greed days (-16,562 USD) is worse than Fear days, suggesting overconfidence |
| **Frequent traders outperform on Greed** | High trade frequency + Greed sentiment = best win rate combination |
| **Consistent winners are sentiment-resilient** | They maintain positive avg PnL even on Fear days unlike inconsistent traders |

---

## Strategy Recommendations

### Strategy 1 — Fear Days: Reduce Size, Keep Frequency
> During **Fear / Extreme Fear** days, reduce individual position size by 30–40% but maintain trade frequency.
- Reasoning: Drawdown is highest on Fear days but avg PnL remains positive — smaller positions limit downside while still capturing upside

### Strategy 2 — Greed Days: Winners Go Long, Inconsistent Traders Stay Neutral
> During **Greed / Extreme Greed** days, Consistent Winner traders should increase long bias and position size. Inconsistent traders should reduce exposure.
- Reasoning: Consistent winners thrive on Greed days. Inconsistent traders have their worst drawdowns during Greed, likely due to overconfidence and poor entries

---

## Notes & Limitations

- **Leverage column** was not present in the dataset — leverage distribution analysis was skipped
- Long/short ratio uses `+1` smoothing to avoid division by zero on days with no sell trades
- Drawdown is a proxy (cumulative PnL based) — not a true peak-to-trough price drawdown
- The trader data covers a specific date range — results may differ across different market cycles

---

*Submitted by: Nikhil Pandey*  
*Assignment: Primetrade.ai Data Science Intern — Round 0*
