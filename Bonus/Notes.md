# Bonus — Charts and Visualization
## Netflix Stock Price (2002–2022) 

**Date:** 20/04/2026
**Dataset:** Netflix Stock Price (2002–2022) 
**Source:** kaggle.com/datasets/jainilcoder/netflix-stock-price-prediction
**Time spent:** 


---

## 📋 Tasks Completed Today
Task 1 — Candlestick chart (OHLC)
Task 2 — Moving average overlay
Task 3 — Monthly volatility bar chart
Task 4 — Volume vs price combo chart
Task 5 — Annual return comparison

Date range: February 2018 — February 2022
Total rows: 1,010 trading days

---

### Task 1 — Candlestick Chart (OHLC)

Built a stock candlestick chart showing Open,
High, Low and Close prices for the last 90
trading days of Netflix stock data.


Dynamic data extraction formula:
=FILTER(NetflixData[[Date]:[Close]],
NetflixData[Date]>=LARGE(NetflixData[Date],90))

How it works:
LARGE(NetflixData[Date],90) finds the 90th
largest date in the dataset — which is the
cutoff date for the last 90 trading days.
FILTER returns only rows where Date is greater
than or equal to that cutoff date.
Result: always returns the most recent 90 rows
dynamically — no manual row selection needed.

This is better than manually copying rows because:
- Updates automatically if new data is added
- No risk of selecting wrong rows
- Formula is self-documenting and auditable

Chart type: Open-High-Low-Close Stock Chart
Built using:
Insert → Charts → All Charts →
Stock → Open-High-Low-Close

Column order required for chart:
Date, Open, High, Low, Close
(Excel requires this exact order for OHLC chart)

Formatting applied:
- Up days (Close > Open) → Green fill
- Down days (Close < Open) → Red fill
- Gridlines removed
- Date axis formatted as MM/DD/YYYY
- Chart title: Netflix Stock Candlestick Chart (Last 90 Days)

Key observation:
Green bars = price closed higher than it opened
Red bars = price closed lower than it opened
Wicks above and below bars show the High
and Low prices for that trading day —
the full range of price movement intraday.

What I learned:
The candlestick chart is the standard chart
used by stock traders and financial analysts
worldwide. Understanding how to read it is
as important as knowing how to build it.
Up = bullish (buyers in control)
Down = bearish (sellers in control)
Long wicks = high uncertainty/volatility
