# Day 5 — XLOOKUP, IF Logic & SUMIFS
## Superstore Sales Dataset

**Date:** April 2, 2026
**Dataset:** Superstore Sales (9,994 rows, 21 columns)
**Continuation of:** Day 3 — same dataset, deeper analysis
**Time spent:** 6:15:38

---

## 📋 Columns Available
Row ID, Order ID, Order Date, Ship Date, Ship Mode,
Customer ID, Customer Name, Segment, Country, City,
State, Postal Code, Region, Product ID, Category,
Sub-Category, Product Name, Sales, Quantity,
Discount, Profit

---

## ✅ Tasks Completed

### Task 1 — Customer Profile Card

Built a dynamic customer profile card on a new sheet.
Type or select a Customer Name from a dropdown and all
fields auto-populate using XLOOKUP with search_mode=-1
to always return the most recent order.

Dataset adjustment: No standalone Customer Name lookup
needed — used Customer Name as primary key. Product Name
lives in a separate Products sheet so built a nested
XLOOKUP to bridge both sheets.

Formulas used:

Last Order Date:
=XLOOKUP(B1,Orders[Customer Name],Orders[Order Date],
"Not Found",0,-1)

Product Name (nested XLOOKUP across 2 sheets):
=XLOOKUP(XLOOKUP(B1,Orders[Customer Name],
Orders[Product ID],"Not Found",0,-1),
Products[Product ID],Products[Product Name],"Not Found")

Sales Amount:
=XLOOKUP(B1,Orders[Customer ID],Orders[Sales],
"Not Found",0,-1)

Region:
=XLOOKUP(B1,Orders[Customer ID],Orders[Region],
"Not Found",0,-1)

Key learning: Applied nested XLOOKUP independently
without prompting — same logic as Day 3 nested VLOOKUP
but across two sheets simultaneously. This shows concept
transfer between days.

Data Validation dropdown:
=UNIQUE(Orders[Customer Name])
Used as source for dropdown so no manual typing needed.

---

### Task 2 — Performance Scorecard

Added 5 calculated columns to working sheet.
Each order scored on 3 conditions then labeled
based on total score.

Profit Margin helper column:
=[@Profit]/[@Sales]

Sales Points (3 if Sales > $1,000):
=IF([@Sales]>1000,3,0)

Margin Points (2 if Profit Margin > 20%):
=IF([@Profit]/[@Sales]>0.2,2,0)

Discount Points (1 if no discount applied):
=IF([@Discount]=0,1,0)

Total Score:
=[@[Sales Points]]+[@[Margin Points]]+[@[Discount Points]]

Performance Label:
=IFS([@[Total Score]]=6,"Star",
[@[Total Score]]>=4,"Good",
[@[Total Score]]>=1,"Average",
TRUE,"No Score")

Results verified using COUNTIF:
=COUNTIF(Orders[Label],Sheet2!B15)

Results:
Star     →  177  orders  (1.8%)
Good     →  97   orders  (1.0%)
Average  →  9,720 orders (97.3%)
Total    →  9,994 orders ✅

Key finding: Only 2.7% of all orders qualify as Star
or Good performers. 97.3% are Average — meaning most
orders have either low sales value, thin margins, or
discounts applied.

Business insight: The business should investigate how
to move more orders into Star and Good categories by
reducing unnecessary discounting and focusing sales
effort on higher value orders.

---

### Task 3 — Regional Target Tracker

Built a target table on Summary sheet with targets
then used SUMIF for actuals and IFS for status flags.

Target table:
Central  → $200,000
East     → $300,000
West     → $250,000
South    → $180,000

Actual Sales formula:
=SUMIF(Orders[Region],A2,Orders[Sales])

Gap % formula:
=(Actual-Target)/Target

Status formula:
=IFS(GapCell>0.1,"Exceeded",
GapCell>=-0.1,"On Track",
TRUE,"Behind")

Results:
Region   Target      Actual        Gap%   Status
Central  $200,000    $501,239.89   151%   Exceeded
East     $300,000    $678,781.24   126%   Exceeded
West     $250,000    $725,457.82   190%   Exceeded
South    $180,000    $391,721.91   118%   Exceeded

Key finding: All 4 regions dramatically exceeded targets.
West had the highest overperformance at 190%.
East had the lowest overperformance at 126%.

Business insight: All targets were set far too
conservatively and do not reflect actual business
capacity. Targets this low remove competitive motivation
from the sales team and make performance tracking
meaningless.

Recommendation: Recalibrate targets using historical
averages — a realistic target would be approximately
80% of actual performance from the prior year.

---

### Task 4 — Multi Condition SUMIFS

Answered 5 business questions using only SUMIFS,
COUNTIFS and AVERAGEIFS formulas.

Dataset adjustment: No quarter column existed so
built one first using:
=("Q"&ROUNDUP(MONTH([@[Order Date]])/3,0))
This converts any date into Q1, Q2, Q3 or Q4
dynamically without hardcoding date ranges.

Q1 — Total sales in Q4 from Technology in West:
=SUMIFS(Orders[Sales],Orders[Category],"Technology",
Orders[Region],"West",Orders[Quarter],"Q4")
Answer: $74,181.81

Q2 — Furniture orders with discount > 0:
=COUNTIFS(Orders[Category],"Furniture",
Orders[Discount],">"&0)
Answer: 1,285 orders

Q3 — Total profit from orders over $500 sales:
=SUMIFS(Orders[Profit],Orders[Sales],">"&500)
Answer: $187,051.71

Q4 — Customers who bought in all 4 regions:
Used UNIQUE to extract customer list then COUNTIFS
to check presence in each region. =UNIQUE(Orders[Customer Name]).
Used COUNTA, UNIQUE, FILTER to count how many regions a
customer has bought from. 
=COUNTA(UNIQUE(FILTER(Orders[Region],Orders[Customer Name]=B20)))
Total Customer was counted using:
=COUNTIF(C20:C812,"="&4)
Answer: 301 customers bought in all 4 regions

Q5 — Average order value by segment:
=AVERAGEIF(Orders[Segment],"Consumer",Orders[Sales])
=AVERAGEIF(Orders[Segment],"Corporate",Orders[Sales])
=AVERAGEIF(Orders[Segment],"Home Office",Orders[Sales])

Results:
Consumer    → $223.73 average order value
Corporate   → $233.82 average order value
Home Office → $240.97 average order value

Key finding: Home Office segment has the highest
average order value at $240.97 — $17 above Consumer.
Corporate falls in the middle at $233.82.

Business insight: Despite being the smallest segment,
Home Office customers spend the most per order on
average. Worth targeting with premium product
promotions to maximize revenue per transaction.

---

### Task 5 — Dynamic Top-N Finder

Built a system where typing N in cell L2
automatically shows the top N customers by
total sales using XLOOKUP + LARGE + spilled range.

Step 1 — Built unique customer summary sheet:
Col A: =UNIQUE(Orders[Customer Name])
Col B: =SUMIF(Orders[Customer Name],A2,Orders[Sales])

Step 2 — Top-N display formula:
=IF(E20:E25<=$L$2,IFERROR(XLOOKUP(LARGE(
D20:D812,E20:E25),D20:D812,B20#,
"Tie/Not Found"),""),"")

Key technique: Used B20# spilled range reference
from UNIQUE formula instead of fixed column reference.
This means the lookup range auto-expands if new
customers are added — making the model dynamic
and production ready.

Top 3 customers by total sales:
1. Sean Miller
2. Tamara Chand
3. Raymond Buch

Maximum N tested: 3
Tested with N = 1, 2, 3 — all returned correct
unique customer names sorted descending. ✅

Known limitation: LARGE returns same value for tied
customers and XLOOKUP returns first match only.
Documented as known constraint — would need a
tiebreaker column to handle perfectly in production.

---

## 💡 Key Formulas Learned Today

### XLOOKUP with search_mode=-1
=XLOOKUP(lookup,lookup_array,return_array,"Not Found",0,-1)
Searches from last row upward — always returns most
recent record. Essential for time-based lookups.

### Nested XLOOKUP across two sheets
=XLOOKUP(XLOOKUP(B1,Orders[Customer Name],
Orders[Product ID],"Not Found",0,-1),
Products[Product ID],Products[Product Name],"Not Found")
Inner XLOOKUP result feeds outer XLOOKUP.
Applied independently using Day 3 nested VLOOKUP concept.

### Quarter generator from date
="Q"&ROUNDUP(MONTH([@[Order Date]])/3,0)
Converts any date to Q1/Q2/Q3/Q4 dynamically.
MONTH extracts month number, dividing by 3 and
rounding up maps it to the correct quarter.
More flexible than hardcoding date ranges.

### LARGE + XLOOKUP for dynamic ranking
=XLOOKUP(LARGE(TotalCol,N),TotalCol,NameCol)
LARGE finds the Nth highest value.
XLOOKUP finds who owns that value.
Together they build a dynamic ranking system.

### Spilled range reference with #
=XLOOKUP(LARGE(D:D,N),D:D,B20#)
B20# references the entire spilled range from a
UNIQUE formula automatically. Expands when new
data is added without updating the formula.

---

## 😤 What Was Hard

Task 5 was the most challenging today.
Had to understand the formula logic before
implementing — specifically how LARGE and XLOOKUP
work together to build a ranking system.

The key breakthrough was understanding that:
- LARGE finds the value at rank N
- XLOOKUP then finds the name that owns that value
- They must work from a UNIQUE customer list not
  the full orders table — otherwise repeated customer
  names cause the same name to appear multiple times

Once that logic clicked the formula came together
naturally. Understanding before implementing is
always better than copying and hoping it works.

---

## 🔄 What I'd Do Differently

1. Build the unique customer summary sheet first
   before attempting Task 5. I tried working from
   the full orders table first and wasted time
   debugging the repeated name issue.

2. Always generate helper columns like Quarter
   at the start of the day before building formulas.
   Having Quarter ready made Task 4 Q1 much simpler
   than using date range conditions.

3. Extend the Top-N system to show total sales
   alongside customer names so the output is more
   useful as a business report.

4. Set more realistic targets in Task 3 from the
   start. All 4 regions exceeding by 100%+ means
   the analysis produced no useful insight about
   which regions are actually underperforming.

---

## 📁 Files in This Folder
- day04_superstore.xlsx
- notes.md

---
