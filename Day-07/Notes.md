# Day 7 — Pivot Tables Part 2 & Dashboard
## Superstore Sales Dataset

**Date:** April 8, 2026
**Dataset:** Superstore Sales (9,994 rows, 21 columns)
**Continuation of:** Day 5 — same dataset, deeper analysis
**Time spent:** 12:45:09

---

## 📋 Tasks Completed Today
1. Executive KPI Dashboard
2. Drill Down Chart System
3. Loss Maker Investigation
4. Customer Segment Funnel
5. Regional Trend Story (West Region)

---

## ✅ Tasks Completed

### Task 1 — Executive KPI Dashboard

Built 4 KPI cards at the top of dashboard sheet
linked to pivot grand total cells using = references.
Cards update automatically when any slicer changes.

Dashboard setup:
- Gridlines turned off
- Sheet tab colored dark blue
- Professional teal blue color scheme throughout

KPI Results:
Total Revenue  → $2,297,200.86
Total Profit   → $286,397.02
Total Margin   → 12%
MoM Growth     → Jun -1%

Slicers added and connected to ALL pivots:
- Category slicer (Furniture, Office Supplies, Technology)
- Region slicer (Central, East, South, West)
- Segment slicer (Consumer, Corporate, Home Office)
- Year slicer at top right (2014, 2015, 2016, 2017)

All 4 slicers connected to every pivot table
using Report Connections so entire dashboard
updates from a single slicer click.

How slicers were connected:
Right click slicer → Report Connections →
ticked all pivot tables

Key design decision: Year slicer placed
horizontally at top right to save vertical
space — a professional layout choice that
keeps the dashboard clean and uncluttered.

---

### Task 2 — Drill Down Chart System

Built two linked bar charts:
- Chart 1: Sales by Category (high level view)
- Chart 2: Sales by Sub-Category (detail view)

Both charts connected to the same Category slicer.
Clicking a category in the slicer filters both
charts simultaneously creating a drill-down effect.

All charts on dashboard are connected to all
slicers making the entire dashboard fully
dynamic and interactive.

Sales by Category results:
Technology      → $836K
Office Supplies → $719K
Furniture       → $742K

Sales by Sub-Category top results:
Phones     → $330.0K
Chairs     → $328.4K
Storage    → $223.8K
Tables     → $207.0K
Binders    → $203.4K
Machines   → $189.2K

Key finding: Phones and Chairs are the top
two sub-categories by revenue — both nearly
equal at $330K and $328K respectively.
Despite Tables having $207K in revenue it
is a loss-maker as revealed in Task 3.

---

### Task 3 — Loss Maker Investigation

Extended Day 5 bonus task with root cause analysis.
Filtered pivot to negative profit values only:
Right click → Value Filters → Less Than → 0

Loss-making Sub-Categories:
Tables     → -$17,725.48 (worst offender)
Bookcases  → -$3,472.56
Supplies   → -$1,189.10
Total Loss → -$22,387.14

Root cause investigation:
Compared average discount rates between
loss-making and profitable sub-categories.

Finding: HIGH DISCOUNTS are the primary driver
of losses across all 3 loss-making sub-categories.
Tables has significantly higher average discount
than profitable sub-categories like Chairs.

The business is discounting Tables so heavily
that it cannot recover costs — generating
$207K in revenue but losing $17.7K in profit.

Business insight: Tables generates $206,965
in revenue but loses $17,725 — a -8.6% margin.
This is not a demand problem — Tables sells well.
It is a pricing and discounting problem.

Recommendation:
1. Immediately cap Tables discount at 10% maximum
2. Review Bookcases and Supplies discount policies
3. Monitor margin weekly after discount cap
4. If margin does not improve consider repricing
   or discontinuing the product line entirely

Chart built: Horizontal red bar chart sorted
ascending (worst to best) so Tables appears
at bottom as most visually impactful loss-maker.
Red color chosen deliberately to signal danger.

---

### Task 4 — Customer Segment Funnel

Built a 4-stage funnel showing how many orders
pass through each profitability threshold.

Stage formulas:

Total Orders:
=COUNTA(Orders[Order ID])

Profitable Orders (Profit > 0):
=COUNTIF(Orders[Profit],">"&0)

High Margin Orders (Margin > 20%):
=COUNTIFS(Orders[Profit],">"&0,
[Profit Margin],">"&0.2)

High Value Orders (Sales > $500 AND Profit > 0):
=COUNTIFS(Orders[Sales],">"&500,
Orders[Profit],">"&0)

Funnel Results:
Stage                    Count    Drop-off
Total Orders             9,994    —
Profitable Orders        8,058    19.4% lost
High Margin (>20%)       5,896    26.8% lost
High Value (>$500)       457      92.3% lost

Key finding: The biggest drop-off is between
High Margin orders (5,896) and High Value
orders (457) — a 92.3% drop. This means very
few orders are simultaneously high value,
profitable AND above $500 in sales.

Business insight: The business has many small
profitable orders but very few large high-value
profitable orders. Growing the High Value
segment from 457 to even 1,000 orders would
significantly impact overall profitability.

Recommendation: Target Corporate and Home Office
segments (highest average order values) with
premium product bundles to increase order value
while maintaining healthy margins.

---

### Task 5 — West Region Sales Trend & Growth

Built a combo chart for West region showing:
- Monthly sales as blue clustered column bars
- Cumulative running total as orange line
  on secondary axis

Pivot setup:
Filter: Region = West
Rows: Order Date grouped by Month
Values: Sum of Sales + Running Total

Running total added using:
Value Field Settings → Show Values As →
Running Total In → Order Date

Combo chart setup:
Monthly Sales = Clustered Column (primary axis)
Cumulative Total = Line (secondary axis)

Results:
Peak month:          November → $352.5K
Second highest:      December → $325.3K
Weakest month:       February (consistently low)

Cumulative total by December: reached 2,500K
on secondary axis showing strong West region
full year performance.

Annotation added: orange arrow pointing to
November bar with text "Peak Month: Nov $352.5K"

Key finding: November is the clear peak month
for West region — likely driven by pre-holiday
and Black Friday purchasing patterns.
September is also strong suggesting back-to-school
and end-of-quarter business purchasing.

Business insight: West region sales are highly
seasonal with Q4 (Oct-Dec) being critical.
Marketing investment and inventory should be
front-loaded into Q3 preparation (Jul-Sep)
to maximize Q4 peak performance.

---

## 📊 Dashboard Summary

Final dashboard contains:
- 4 KPI cards (Revenue, Profit, Margin, MoM Growth)
- 4 slicers (Category, Region, Segment, Year)
- 5 charts (Category bar, Sub-Category bar,
  Loss Drivers red bar, Customer Funnel,
  West Region combo chart)
- All charts connected to all slicers
- Professional teal blue color scheme
- Gridlines removed
- Year slicer horizontal at top right

Dashboard rating: 9.5/10

Strengths:
- Fully interactive — every element responds to slicers
- 4 different chart types showing versatility
- Red loss driver bars immediately signal danger
- Combo chart with cumulative trend is advanced
- 3 slicers + year filter gives full analytical control

---

## 💡 Key Skills Learned Today

### Connecting slicers to multiple pivots
Right click slicer → Report Connections →
tick all pivot tables.
One slicer click filters every chart and KPI
simultaneously — essential for interactive dashboards.

### KPI cards linked to pivot grand totals
= reference to pivot grand total cell.
Card updates automatically when pivot filters change.
Nothing hardcoded — fully dynamic.

### Running Total in pivot
Value Field Settings → Show Values As →
Running Total In → [date field]
Creates cumulative total without any formula.
Used for West region growth trend line.

### Combo chart (Column + Line)
Insert → Combo Chart →
Series 1 = Clustered Column
Series 2 = Line on Secondary Axis
Two metrics on same chart telling one story.

### Value Filter for negatives only
Right click label → Value Filters → Less Than → 0
Isolates loss-making rows instantly.
Used for Loss Driver investigation.

### Red bars for loss visualization
Split data into positive and negative series.
Format negative series → red fill.
Visual immediately communicates danger
without the reader needing to read values.

### Dashboard design thinking
Plan layout on paper before building in Excel.
Group related charts together logically.
Use consistent color scheme throughout.
Every element should earn its place on the dashboard.

---

## 😤 What Was Hard

Most tasks required proper thinking and planning
before building — not just formula knowledge.

Dashboard design was the hardest part.
Deciding where to place 5 charts, 4 KPI cards
and 4 slicers without it looking cluttered
required sketching the layout first.

The combo chart for Task 5 required understanding
how secondary axes work and why the cumulative
line needed its own axis — otherwise both series
would share the same scale making the monthly
bars invisible against the large cumulative numbers.

---

## 🔄 What I'd Do Differently

1. Sketch the full dashboard layout on paper
   before opening Excel — same lesson as Day 4
   but applies even more strongly here with
   5 charts to position simultaneously.

2. Add business insights text box from the start
   as a placeholder — fill in the findings as
   each chart is built rather than leaving it
   for the end.

3. Investigate the MoM Growth formula earlier
   to make sure it is dynamic and not hardcoded
   before finalizing the dashboard.

4. Test all slicer connections immediately after
   connecting them — clicking each slicer option
   to confirm every chart responds correctly
   before moving on to the next task.

---

## 📁 Files in This Folder
- day07_superstore_dashboard.xlsx
- day07_dashboard_screenshot.png
- notes.md

---
