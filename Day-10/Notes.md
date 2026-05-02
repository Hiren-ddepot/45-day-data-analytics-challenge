# Day 10 — Full Dashboard Capstone Project
## Superstore Sales Dataset — Power Query + Data Model + DAX

**Date:** May 2, 2026  
**Dataset:** Superstore Sales (9,994 rows, 21 columns)  
**Approach:** Modern BI structure using Power Query,
Data Model and DAX — not traditional Excel formulas  
**Time spent:** 05:28:11  

---

## 📋 Project Overview

This project transforms raw Superstore sales data
into a professional analytical model using Excel's
modern BI stack:

Power Query → Data Cleaning and Feature Engineering  
Data Model  → Relationships and Business Logic  
DAX         → Advanced Metrics and Calculations  
PivotTables → Analysis Layer  
Dashboard   → Executive Reporting Layer  

This is not a traditional Excel project.
This is a Business Intelligence solution
built inside Excel — the same architecture
used in Power BI and enterprise BI systems.

---

## 🗄️ Data Architecture

### Data Model Design

Split the raw dataset into 3 normalized tables
following a star schema structure:

Orders Table (Fact Table)  
Primary key: Order ID  
Foreign keys: Customer ID, Product ID  
Contains: Rows ID, Order Date, Ship Date, Profit, Discount,
Sales, Quantity, Ship Mode

Customers Table (Dimension Table)  
Primary key: Customer ID  
Contains: Customer Name, Segment, Country,
City, State, Postal Code, Region

Products Table (Dimension Table)  
Primary key: Product ID  
Contains: Product Name, Category, Sub-Category

Relationships:  
Orders[Customer ID] → Customers[Customer ID]  
Orders[Product ID]  → Products[Product ID]  

Why this structure:
- Eliminates data redundancy
- Each piece of information stored exactly once
- Enables DAX to calculate across related tables
- Same architecture used in Power BI and SQL databases
- More scalable than a flat single table approach

This is how real business data is structured
in enterprise systems. Building it this way
from Day 10 shows architectural thinking
beyond basic Excel skills.

---

## 🧹 Data Preparation (Power Query)

All data cleaning and feature engineering
performed in Power Query before loading
into the Data Model.

Transformations performed:

1. Date standardization  
   Converted and standardized all date fields
   to ensure consistent date format throughout.

2. Order Month-Year column  
   Created for time-based trend analysis.
   Used for monthly Sales Trend chart on dashboard.

3. Days to Ship column  
   Calculated delivery speed:
   Days to Ship = Ship Date - Order Date
   Measures logistics and shipping efficiency.

4. Error handling  
   Used try...otherwise logic to handle null
   values and prevent formula errors propagating
   through the model.

5. Data type correction  
   Ensured all columns have correct data types:
   dates as dates, numbers as numbers,
   text as text — essential for DAX to work correctly.

Key Power Query lesson:  
Power Query transformations are non-destructive.
The original data is never modified — every
step is recorded and can be edited or removed.
This is the gold standard for data preparation
because every transformation is auditable
and reproducible.

---

## 🧠 DAX Calculations (Data Model)

Three DAX measures built in the Data Model:

### Measure 1 — Profit Margin %
Measures profitability per transaction.

DAX formula:  
Profit Margin % =
DIVIDE(Orders[Total Profit], Orders[Total Sales], 0)

Why DIVIDE not /:  
DIVIDE handles division by zero gracefully.
If Total Sales = 0 it returns 0 instead of error.
Always use DIVIDE for ratio calculations in DAX.

### Measure 2 — Customer Lifetime Value (CLV Proxy)
Calculates total revenue generated per customer
across ALL their transactions — not just current filter.

DAX formula:  
CLV =
CALCULATE(
    SUM(Orders[Sales]),
    ALLEXCEPT(Orders, Orders[Customer ID])
)  

How it works:  
CALCULATE modifies the filter context.
ALLEXCEPT removes all filters EXCEPT Customer ID.
Result: for each row it calculates total sales
for that customer regardless of other filters.

Business use:  
Identifies high-value customers.  
Sean Miller → $25,043 total lifetime value.  
Tamara Chand → $19,052 total lifetime value.  

### Measure 3 — Profitability Tier
Classifies each transaction into performance tier
based on profit margin percentage.

DAX formula:  
Profitability Tier =
SWITCH(
    TRUE(),
    [Profit Margin %] > 0.2, "High",
    [Profit Margin %] > 0.05, "Medium",
    "Low"
)  

How SWITCH(TRUE()) works:  
Evaluates each condition in order.  
First condition that is TRUE wins. 
Cleaner than nested IF for multiple conditions.  
Same logic as IFS in regular Excel but in DAX.

Tiers defined:  
High   → Margin > 20%  
Medium → Margin 5-20%  
Low    → Margin < 5% (includes losses)  

Business use:  
Enables segmentation of profitable vs
low-performing orders across any dimension.  

---

## 📊 Dashboard Results

### KPI Cards
Total Sales:    $2.30M  
Total Profit:   $286.40K  
Profit Margin:  12.47%  
Order Count:    9,994  

### Chart 1 — Total Sales by Region
West    → $0.76M (highest) ✅  
East    → $0.61M  
Central → $0.52M  
South   → $0.40M (lowest)  

West leads all regions by $150K over East.  
South is significantly behind at nearly half
of West region revenue.

### Chart 2 — Sales Trend by Month
Clear upward trend from 2014 to 2017.  
Noticeable spikes suggest seasonal demand
and promotional impact — particularly in
September and November each year.  
Monthly granularity reveals patterns that
annual totals would hide.

### Chart 3 — Top 10 Customers by Revenue
Sean Miller        → $25,043.05  
Tamara Chand       → $19,052.22  
Raymond Buch       → $15,117.34  
Tom Ashbrook       → $14,595.62  
Adrian Barton      → $14,473.57  
Ken Lonsdale       → $14,175.23  
Sanjit Chand       → $14,142.33  
Hunter Lopez       → $12,873.30  
Sanjit Engle       → $12,209.44  
Christopher Conant → $12,129.07  

Sean Miller is 31% ahead of second place
Tamara Chand — a significant concentration
of value in one customer.

### Chart 4 — Sub-Category Profitability
Combo chart: Total Profit (bars) +
Profit Margin % (orange line) on secondary axis.

Best performers:  
Copiers → highest profit + high margin  
Phones  → high profit + good margin  

Worst performers:  
Tables    → negative profit + negative margin  
Bookcases → negative profit  
Supplies  → negative profit  

Notable finding:  
Machines have high sales but very low margins —
generating revenue without proportionate profit.

### Slicers
Year slicer:     2014, 2015, 2016, 2017  
Region slicer:   Central, East, South, West  
Category slicer: Furniture, Office Supplies, Technology  

All slicers connected to all pivots and charts.  
Entire dashboard updates from single slicer click.  

---

## 💡 Key Business Insights

1. Total Sales reached $2.3M with $286K profit
   and 12.4% margin across 9,994 orders.
   The business is profitable but margin is thin —
   leaving little room for cost increases.

2. West region leads revenue at $0.76M —
   nearly double South region at $0.40M.
   South region needs investigation to understand
   whether it is a market size or execution issue.

3. Noticeable sales spikes in September and
   November suggest seasonal demand and
   promotional impact. The business should
   plan inventory and marketing around these
   peak months to maximize revenue capture.

4. Copiers and Phones drive the highest profit
   and should be prioritized in sales strategy.
   Technology category overall is the most
   profitable per unit sold.

5. Tables, Bookcases and Supplies generate losses
   despite meaningful revenue. Root cause identified
   in earlier analysis as excessive discounting.
   Recommend immediate discount caps on these
   sub-categories.

6. Machines have high sales but very low margins —
   a hidden profitability problem. High revenue
   does not equal high value. Margins need
   monitoring and pricing review.

---

## 💡 Key Skills Learned Today

### Star Schema Data Modeling
Splitting flat data into fact and dimension tables.  
Orders = fact table (transactions)  
Customers, Products = dimension tables (entities)  
Connected via primary and foreign keys.  
Foundation of every enterprise BI system.  

### Power Query ETL Pipeline
Extract → Transform → Load workflow.  
Non-destructive transformations — original 
data never modified.  
Every step recorded, auditable and reproducible.
try...otherwise for error-resistant pipelines.

### DAX DIVIDE function
=DIVIDE(numerator, denominator, alternate)  
Handles division by zero gracefully.  
Always use DIVIDE not / for ratio calculations.

### DAX CALCULATE + ALLEXCEPT
CALCULATE modifies filter context.  
ALLEXCEPT removes all filters except specified.  
Used for Customer Lifetime Value calculation
that ignores current row filters.

### DAX SWITCH(TRUE())
Evaluates multiple conditions in order.  
First TRUE condition wins.  
DAX equivalent of Excel IFS function.  
Cleaner than nested IF for multiple conditions.  

### Combo chart with dual axis
Two metrics on same chart telling one story.  
Profit bars + Margin line reveals that
high revenue does not always mean high margin —
a story impossible to tell with one chart type.

---

## 😤 What Was Hard

Nothing was technically difficult in terms
of Excel skills — Days 1-9 built a strong
enough foundation that Day 10 felt manageable.

The challenge was working differently.
Using Power Query and Data Model instead of
traditional Excel formulas required a mindset
shift — thinking about data architecture first
rather than jumping straight into formulas.

Learning DAX syntax (CALCULATE, ALLEXCEPT,
DIVIDE) took time because it behaves differently
from Excel formulas. DAX thinks in filter
contexts not cell references.

The time investment was significant but the
result — a proper BI solution — was worth it.

---

## 🔄 What I'd Do Differently

In practice context I would not do anything
differently — this was a learning exercise
and pushing into Power Query and DAX was
the right decision for maximum learning.

Under real work conditions I would choose
the simplest approach that generates the
same quality result. If traditional pivot
tables achieve the same dashboard output
for a one-time report — use pivot tables.

If the report runs weekly and needs to refresh
automatically — use Power Query and Data Model.

The right tool depends on the use case.
Not every problem needs the most sophisticated
solution. A good analyst matches tool complexity
to problem complexity.

---

## 📁 Files in This Folder
- day10_superstore_dashboard.xlsx
- day10_dashboard_screenshot.png
- notes.md

---

## 🔗 LinkedIn Post
[paste your LinkedIn post URL after posting]

---

## 🏆 Excel Phase Complete

10 days. 10 real datasets. 10 documented projects.

Skills acquired:
✅ Formulas — IF, SUMIFS, XLOOKUP, INDEX/MATCH  
✅ Text and date functions  
✅ Data cleaning — 119K rows cleaned  
✅ Pivot tables and calculated fields  
✅ Interactive dashboards with slicers  
✅ Advanced functions — LARGE, FILTER, UNIQUE    
✅ Power Query — ETL pipeline  
✅ Data Model — star schema architecture   
✅ DAX — CALCULATE, ALLEXCEPT, DIVIDE, SWITCH  

Starting point: basic Excel knowledge
Ending point:   junior data analyst portfolio

Next phase: SQL — Days 11-22
