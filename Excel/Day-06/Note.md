# Day 5 — Pivot Tables Part 1
## Superstore Sales Dataset

**Date:** 03/04/2026
**Dataset:** Superstore Sales (9,994 rows, 21 columns)
**Continuation of:** Days 3 & 4 — same dataset, deeper analysis
**Time spent:** 6:11:32

---

## 📋 Tasks Completed Today
1. Profitability Matrix (Category × Region)
2. Year-over-Year Sales Trend
3. Customer RFM Segmentation
4. Shipping Efficiency Pivot
5. Loss Maker Dashboard (bonus task)

---

## ✅ Tasks Completed

### Task 1 — Profitability Matrix

Built a pivot table with Category in rows,
Region in columns, Profit Margin as calculated
field (=Profit/Sales). Applied color scale
conditional formatting — green for high margin,
red for low.

Calculated field formula:
=Profit/Sales

Results:
             Central    East      South     West
Furniture    -$89.39    $55.53    $47.03    $69.09
Office Sup   -$225.93   $353.20   $162.35   $542.15
Technology   $73.55     $67.54    $55.52    $91.76

Key findings:
Worst margin: Office Supplies Central → -$225.93
Best margin:  Office Supplies West → $542.15

Business insight: The Central region is a serious
concern — both Furniture and Office Supplies have
negative profit margins there. Only Technology
is profitable in Central. This suggests a Central
region cost or pricing problem that needs urgent
investigation.

Recommendation: Conduct a deep dive into Central
region operations — are discounts too high there?
Are shipping costs eating into margins? Is pricing
competitive but costs too high?

Correction made during task:
Initially jumped into building without understanding
what a calculated field was. Paused, read the
requirement fully, then built it correctly.
Lesson: always understand before jumping in.

---

### Task 2 — Year-over-Year Sales Trend

Built a pivot with Month in rows and Year in columns.
Added YoY growth % calculated outside the pivot
using formula:
=(ThisYear-LastYear)/LastYear

Grouped Order Date by Month and Year in pivot.

Results:
Month  2014        2015        2016        2017
Jan    $14,236.90  $18,174.08  $18,542.49  $43,971.37
Feb    $4,519.89   $11,951.41  $22,978.82  $20,301.13
Mar    $55,691.01  $38,726.25  $51,715.88  $58,872.35
Apr    $28,295.35  $34,195.21  $38,750.04  $36,521.54
May    $23,648.29  $30,131.69  $56,987.73  $44,261.11
Jun    $34,595.13  $24,797.29  $40,344.53  $52,981.73
Jul    $33,946.39  $28,765.33  $39,261.96  $45,264.42
Aug    $27,909.47  $36,898.33  $31,115.37  $63,120.89
Sep    $81,777.35  $64,595.92  $73,410.02  $87,866.65
Oct    $31,453.39  $31,404.92  $59,687.75  $77,776.92
Nov    $78,628.72  $75,972.56  $79,411.97  $118,447.83
Dec    $69,545.62  $74,919.52  $96,999.04  $83,829.32

YoY Growth highlights:
Best growth:  February 2016 → +408.4%
Worst month:  March 2015 → -30.5%
Biggest jump: January 2017 → +208.9%

Key findings:
1. September and November are consistently the
   strongest months across all years — likely
   driven by back to school and pre-holiday
   purchasing patterns.

2. February is consistently the weakest month
   but showed explosive 408% growth in 2016 —
   worth investigating what drove that spike.

3. Overall the business is growing year on year
   with 2017 being the strongest year across
   almost every month.

Business insight: The business has clear seasonal
peaks in September and November. Marketing and
inventory planning should prioritize these months
to maximize revenue during peak demand periods.

---

### Task 3 — Customer RFM Segmentation

Built RFM analysis showing each customer's:
- Recency: days since last order (TODAY - MAX order date)
- Frequency: count of orders
- Monetary: total sales

Correction made during task:
Initial recency formula calculated from overall
dataset MAX date giving every customer the same
value of 3,095 days. Fixed using:
=TODAY()-MAXIFS(Orders[Order Date],
Orders[Customer Name],A2)

This calculates recency per customer individually.

Top customers by monetary value:
Customer              Freq  Total Sales   Recency
Sean Miller           15    $25,043.05    3,095 days
Tamara Chand          12    $19,052.22    3,415 days
Raymond Buch          18    $15,117.34    3,112 days
Tom Ashbrook          10    $14,595.62    3,085 days
Adrian Barton         20    $14,473.57    3,057 days
Ken Lonsdale          29    $14,175.23    3,063 days
Sanjit Chand          22    $14,142.33    3,365 days
Hunter Lopez          11    $12,873.30    3,059 days
Sanjit Engle          19    $12,209.44    3,025 days
Christopher Conant    11    $12,129.07    3,059 days
Todd Sumrall          15    $11,891.75    3,052 days
Greg Tran             29    $11,820.12    3,052 days
Becky Martin          16    $11,789.63    3,323 days
Seth Vernon           32    $11,470.95    3,117 days

Key findings:
Highest monetary:  Sean Miller → $25,043.05
Highest frequency: Seth Vernon → 32 orders
Most recent:       Sanjit Engle → 3,025 days ago

Note: High recency days (3,000+) across all
customers reflects that the dataset ends in 2017.
These are not real-time recency values — they
represent days from last order to today's date.
In a real business context recency would be
calculated relative to the last date in the dataset
not TODAY() to give meaningful segmentation.

Business insight: Sean Miller is the highest value
customer by far at $25,043 — nearly $6,000 ahead
of second place. Seth Vernon places the most orders
at 32 but is not the highest spender suggesting
many small purchases. High value + high frequency
customers like Ken Lonsdale (29 orders, $14,175)
represent the most loyal customer segment.

---

### Task 4 — Shipping Efficiency Pivot

Added Ship Days helper column to working sheet:
=Orders[Ship Date]-Orders[Order Date]

Built pivot: Ship Mode × Category showing
Average Ship Days and Sum of Profit.

Results:
Ship Mode       Category          Avg Days  Profit
First Class     Furniture         2.14      $3,066.95
                Office Supplies   2.20      $18,400.33
                Technology        2.16      $27,502.56
First Class Total                 2.18      $48,969.84

Same Day        Furniture         0.03      $797.35
                Office Supplies   0.05      $6,423.52
                Technology        0.05      $8,670.89
Same Day Total                    0.04      $15,891.76

Second Class    Furniture         3.26      $4,226.26
                Office Supplies   3.22      $27,068.17
                Technology        3.28      $26,152.21
Second Class Total                3.24      $57,446.64

Standard Class  Furniture         4.98      $10,360.72
                Office Supplies   5.02      $70,598.78
                Technology        4.98      $83,129.29
Standard Class Total              5.01      $164,088.79

Key findings:
Fastest ship mode:      Same Day (0.04 avg days)
Most profitable mode:   Standard Class ($164,088.79)
Least profitable mode:  Same Day ($15,891.76)

Business insight: Same Day shipping is the fastest
but generates the least profit at $15,891.76 —
less than 10% of Standard Class profit. Standard
Class despite being the slowest at 5 days generates
$164,088.79 — over 10x more than Same Day.

This does not necessarily mean Same Day is bad —
it likely serves a different customer need. But
the business should ensure Same Day premium pricing
reflects the operational cost of fast fulfillment.

---

### Task 5 — Loss Maker Dashboard (Bonus)

Found all Sub-Categories with negative total profit.
Built a horizontal bar chart showing only loss-makers
sorted worst to best (ascending).
Added 3 KPI cards linked to filtered pivot totals.

Loss-making Sub-Categories:
Sub-Category    Total Profit
Tables          -$17,725.48
Bookcases       -$3,472.56
Supplies        -$1,189.10

KPI Cards:
Total Loss Amount        → -$22,387.14
Loss-Making Sub-Cats     → 3
Worst Offender           → Tables (-$17,725.48)

How I filtered pivot to negatives only:
Right clicked Sub-Category label →
Value Filters → Less Than → 0

Chart: Horizontal bar chart sorted ascending
(worst to best) so Tables appears at bottom
as most visually impactful loss-maker.

Key findings:
1. Tables alone accounts for 79% of all losses
   at -$17,725.48 — a critical problem product
2. All 3 loss makers are in the Furniture category
   — confirms the Furniture profitability problem
   identified in Task 1
3. Combined loss of -$22,387.14 across only 3
   sub-categories represents a significant drag
   on overall business profitability

Business insight: Tables is actively destroying
value. The business generates $206,965 in Tables
revenue but loses $17,725 — a -8.6% margin.
This is not a margin problem — it is a structural
pricing or cost problem.

Recommendation: Either reprice Tables to achieve
positive margins, renegotiate supplier costs, or
discontinue the product line entirely. Continuing
to sell Tables at current pricing is worse than
not selling them at all.

---

## 💡 Key Skills Learned Today

### Pivot Calculated Field
PivotTable Analyze → Fields Items & Sets →
Calculated Field → =Profit/Sales
Creates a new metric inside the pivot that
calculates row by row automatically.

### Grouping dates in pivot
Right click any date in pivot → Group →
Select Months and Years simultaneously.
Creates a Month × Year matrix automatically.

### Color scale conditional formatting on pivot
Select all value cells in pivot →
Home → Conditional Formatting → Color Scales.
Red = low, Green = high. Visual at a glance.

### RFM Analysis
Recency = TODAY() - MAXIFS(DateCol,NameCol,Customer)
Frequency = COUNTIF(NameCol, Customer)
Monetary = SUMIF(NameCol, Customer, SalesCol)
Three metrics that together identify your most
valuable customers.

### Value filter on pivot (negatives only)
Right click label → Value Filters → Less Than → 0
Shows only rows where the value meets condition.
Used to isolate loss-making sub-categories.

### Ship Days helper column
=Ship Date - Order Date
Excel stores dates as numbers so subtracting
two dates gives the number of days between them.

---

## 😤 What Was Hard

Understanding what each task was asking before
starting to build. I found myself wanting to jump
straight into Excel before fully reading the
requirement.

The RFM task was a good example — I built the
recency formula incorrectly the first time because
I did not fully think through what recency means
per customer vs across the whole dataset.

The calculated field in Task 1 also required
pausing to understand what a calculated field
actually is before I could build it correctly.

---

## 🔄 What I'd Do Differently

Always understand before jumping in.

This is my biggest lesson from Day 6. Every time
I read a task fully, thought about what the output
should look like, and planned my approach first —
the build was smooth. Every time I jumped straight
in — I had to redo work.

Going forward my process for every task:
1. Read the full task twice
2. Identify what the output should look like
3. Identify which tool or formula achieves that
4. Build only after steps 1-3 are complete

This is not just an Excel lesson. This is how
professional analysts approach every problem.
Understand the question before answering it.

---

## 📁 Files in This Folder
- day06_superstore.xlsx
- notes.md

---

## 🔗 LinkedIn Post
