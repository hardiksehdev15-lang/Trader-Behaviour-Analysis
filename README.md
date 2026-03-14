# Trader Behaviour Analysis Using Fear & Greed Index

## Overview

This project analyzes **trader behavior under different market sentiment conditions** using the **Fear & Greed Index** and historical trading data.

The goal is to understand how **market emotions influence trading decisions**, including profitability, leverage usage, trading frequency, and position size.

By combining **sentiment data** with **trade-level historical data**, the project identifies patterns such as:

* How profitability changes during **Fear vs Greed**
* Whether traders **take more leverage in certain sentiments**
* Changes in **win rate, trading frequency, and position size**
* Differences between **high leverage and low leverage traders**

The analysis is implemented in **Python using data analysis and visualization libraries**.

---

# Dataset Description

## 1. Fear & Greed Index Dataset

Contains daily market sentiment classification.

Typical columns include:

| Column         | Description                                                          |
| -------------- | -------------------------------------------------------------------- |
| date           | Date of sentiment record                                             |
| classification | Market sentiment category (Fear, Extreme Fear, Greed, Extreme Greed) |

This dataset represents the **emotional state of the crypto/financial market**.

---

## 2. Historical Trading Data

Contains individual trade records of traders.

Important columns include:

| Column         | Description                   |
| -------------- | ----------------------------- |
| Account        | Trader identifier             |
| Timestamp IST  | Time of trade                 |
| Closed PnL     | Profit or loss from the trade |
| Size USD       | Position size                 |
| Start Position | Initial trade size            |
| Direction      | Trade direction (Long/Short)  |

This dataset helps analyze **real trading behavior**.

---

# Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Jupyter Notebook

---

# Project Workflow

## 1. Data Loading

Both datasets are loaded using **Pandas**.

```python
fear_greed = pd.read_csv("fear_greed_index.csv")
historical = pd.read_csv("historical_data.csv")
```

---

# 2. Data Cleaning

Steps performed:

* Handling missing values
* Removing duplicate rows
* Converting timestamp columns to datetime format

```python
fear_greed['date'] = pd.to_datetime(fear_greed['date']).dt.date
historical['Timestamp IST'] = pd.to_datetime(historical['Timestamp IST'], dayfirst=True)
```

---

# 3. Feature Engineering

Several trading metrics were created:

### Daily Profit & Loss

Total PnL per trader per day.

```python
daily_pnl = historical.groupby(['Account','date'])['Closed PnL'].sum()
```

---

### Win Rate

Percentage of profitable trades.

```python
historical['win'] = historical['Closed PnL'] > 0
```

---

### Average Trade Size

Average position size taken by traders.

```python
avg_trade_size = historical.groupby('Account')['Size USD'].mean()
```

---

### Leverage Intensity

Since actual leverage data was unavailable, a **proxy metric** was created.

Leverage intensity measures **how far a trade's start position deviates from the average**.

```
leverage_intensity = (Start Position − Mean Start Position) / Std Dev
```

This standardization helps identify **high risk vs low risk trades**.

---

# 4. Dataset Merging

Trading data was merged with sentiment data using the **date column**.

```python
merged = historical.merge(fear_greed, on='date', how='left')
```

This allows analyzing **trading behavior under each market sentiment**.

---

# Analysis Performed

## 1. Profitability vs Market Sentiment

Average PnL was calculated for each sentiment category.

Findings:

* Highest profits occurred during **Extreme Greed**.
* Traders appear more aggressive in bullish markets.

---

## 2. Win Rate vs Sentiment

Win rate was compared across sentiment states.

Findings:

* Win rate increased during **Greed and Extreme Greed** periods.

---

## 3. Drawdown Analysis

Loss trades were isolated to understand downside risk.

Metrics used:

* Average loss per sentiment
* Worst loss per sentiment

Findings:

* **Extreme Fear periods captured the largest losses.**

---

## 4. Trading Frequency

Average number of trades per day was analyzed.

Findings:

* Traders execute **more trades during fear-driven markets**.

This may indicate **panic trading behavior**.

---

## 5. Leverage Behavior

Average leverage intensity was compared across sentiments.

Findings:

* Highest leverage during **Extreme Greed**
* Lower leverage during **moderate Greed**

This suggests traders become **aggressive at market extremes**.

---

## 6. Long vs Short Bias

Trades were classified as:

* Long trades
* Short trades

Analysis showed:

* Traders tend to **hold long positions during fear**
* Short selling increases during **greedy conditions**

---

## 7. Position Size Analysis

Average position size was measured by sentiment.

Findings:

* **Largest position sizes occurred during Fear**

This may indicate traders **attempting to average down losses**.

---

# Leverage Group Analysis

Trades were categorized into:

* **Low Leverage (bottom 33%)**
* **High Leverage (top 33%)**

Thresholds were defined using quantiles.

```python
low_leverage_threshold = merged['leverage_intensity'].quantile(0.33)
high_leverage_threshold = merged['leverage_intensity'].quantile(0.67)
```

---

## Key Findings

### Profitability

High leverage traders had:

* Higher **average PnL**

---

### Win Rate

High leverage traders also showed:

* Higher **win rates**

This suggests that **skilled traders may take larger positions confidently**.

---

# Trading Frequency Analysis

Traders were also grouped by:

* Low frequency traders
* Medium frequency traders
* High frequency traders

Findings:

* **PnL did not strongly depend on trading frequency**
* **Win rate was higher for medium and frequent traders**

---

# Visualizations

The project includes multiple plots:

* Profit vs sentiment
* Win rate vs sentiment
* Leverage intensity distribution
* Trade frequency vs sentiment
* Position size vs sentiment
* Leverage group performance

These visualizations help interpret **trader psychology under different market conditions**.

---

# Key Insights

1. Traders perform best during **Extreme Greed markets**.
2. **Extreme Fear markets produce the largest losses**.
3. Traders **increase leverage during bullish sentiment**.
4. **Trading frequency does not guarantee higher profits**.
5. **High leverage traders tend to outperform low leverage traders**.

---

# How to Run the Project

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/trader-behaviour-analysis.git
```

---

### 2. Install Dependencies

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

---

### 3. Run the Notebook

```bash
jupyter notebook TraderBehaviour.ipynb
```

---

# Project Structure

```
TraderBehaviourAnalysis/
│
├── TraderBehaviour.ipynb
├── fear_greed_index.csv
├── historical_data.csv
├── README.md
```

---

# Future Improvements

* Add **Sharpe Ratio and Risk Adjusted Returns**
* Apply **Machine Learning models** to predict trader success
* Use **real leverage data instead of proxy metrics**
* Build a **trader sentiment prediction model**

---

# Author

Developed as part of a **trading behavior analysis project using sentiment data and historical trade records**.

---
