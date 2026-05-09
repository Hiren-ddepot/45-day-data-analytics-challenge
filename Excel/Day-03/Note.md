# Day 3 — VLOOKUP & Lookup Functions

**Date:** 31/03/2026
**Dataset:** Superstore Sales (9,994 rows, 21 columns)
**New dataset:** First day working with Superstore Sales
**Time spent:** 05:13:32

---

## 📋 Dataset Setup
Split Superstore dataset into two tables using Power Query:
- Orders table: 9,994 rows | transaction data
- Products table: 1850 unique rows | product master data

Reason for splitting: Simulates a real database structure
where orders and products are stored in separate tables.
This is how data is structured in real business systems.

---

## ✅ Tasks Completed
### Task 1 — Dynamic Order Invoice

New sheet created: Invoice

Input method: Data Validation dropdown for Order ID
— prevents typos and simulates a real application.

Fields auto-populated via VLOOKUP:
- Customer Name
- Product Name (nested VLOOKUP via Product ID)
- Category (nested VLOOKUP via Product ID)
- Sub-Category (nested VLOOKUP via Product ID)
- Quantity
- Unit Price

Total formula:
=Quantity * Unit Price

Key decision: Excluded discount from Task 1 invoice
because Task 2 builds a proper tiered discount engine.
Keeping each task focused on one job.

Formulas used:
Customer Name:
=VLOOKUP(A3,Order[[#All],[Order ID]:[Product ID]],6,)

Product Name, Category and Sub-Category:
=VLOOKUP(VLOOKUP(Sheet1!A3,Order[[#All],[Order ID]:[Profit]],13,0),
Product[#All],{4,2,3},0)

Quantity and UnitPrice:
=VLOOKUP(A3,Order[[#All],[Order ID]:[Profit]],{15,14},FALSE)

Tested with 5 different Order IDs — all fields populated
correctly via dropdown selection.

What I learned: Data Validation turns a basic lookup
into a user friendly tool. This is the difference between
a formula and an actual application.

### Task 2 — Tiered Discount Engine

Discount table built on Summary sheet:
Furniture        → 15%
Office Supplies  → 10%
Technology       → 20%

New columns added to Working sheet:
- Category Discount
- Discounted Revenue

Formulas used:

Category Discount:
=IFERROR(VLOOKUP([@Category],DiscountTable,2,0),"No Discount")

Discounted Revenue:
=IFERROR([@Sales]*[@Quantity]*(1-VLOOKUP([@Category],
DiscountTable,2,0)),[@Sales]*[@Quantity])

Error handling logic:
- Category found → correct discount applied
- Category not found → "No Discount" shown
- Revenue fallback → full price calculated if no discount

What I learned:
IFERROR is not just about hiding errors — it is about
defining what should happen when something goes wrong.
In real business data not every value is guaranteed to
exist in a reference table so always plan for missing data.

Business logic: If a new category is added to the dataset
in future, the formula automatically falls back to full
price instead of breaking. This makes the model resilient
and production ready.

### Task 3 — Missing Product Audit

Dataset observation: No missing Product IDs existed in
the original Superstore dataset — it is a clean dataset.

Approach: Manually introduced 10 fake Product IDs
(FAKE-001 to FAKE-010) into the Orders sheet to simulate
real world missing data scenarios.

Flag column added to Orders sheet: Product Flag
Formula:
=IFERROR(VLOOKUP([@[Product ID]],
Product[[#All],[Product ID]:[Sub-Category]],2,0),"MISSING")

Summary sheet audit results:
Total Orders           → 9,994
Missing Product Orders → 10 (after introducing fake IDs)
Clean Orders           → 9,984

Verification:
COUNTIF returned 10 after introducing fake IDs ✅
COUNTIF returned 0 after reverting to originals ✅

Missing products extracted using:
=FILTER(Order[Product ID],Order[Product Flag]="MISSING")

Real world relevance: In real business data Product IDs
go missing when:
- Products are discontinued
- Data is migrated from another system
- Products are deleted from the master catalog
This audit formula would catch all those cases automatically.

What I learned:
Clean datasets do not always reflect real world scenarios.
A good analyst knows how to simulate edge cases to test
their formulas even when the data is perfect.
Always verify both directions — that the flag catches
errors AND that it clears when data is fixed.

### Task 4 — Cross Sheet Revenue Summary

Columns used: Sales, Profit, Discount, Category

Method: Added Category helper column to Orders sheet
via VLOOKUP from Products sheet using Product ID.
Built summary table using SUMIF and AVERAGEIF — no pivot.

Formulas used:
Total Revenue:
=SUMIF(Order[Category],"Furniture",Order[Sales])

Total Profit:
=SUMIF(Order[Category],"Furniture",Order[Profit])

Avg Discount:
=AVERAGEIF(Order[Category],"Furniture",Order[Discount])

Profit Margin %:
=Total Profit/Total Revenue

Revenue Share %:
=Category Revenue/SUM(All Revenue)

Results:

Furniture       → $742k revenue | 2% margin  | 32.3% share

Office Supplies → $719k revenue | 17% margin | 31.3% share

Technology      → $836k revenue | 17% margin | 36.4% share

Total Revenue   → $2.297M


Key findings:
1. Technology is the strongest category — highest revenue
   AND highest profit at $145k
2. Furniture is a major concern — $742k revenue but only
   2% profit margin. High discount rate of 17.4% is likely
   the primary cause of margin erosion
3. Office Supplies is the most efficient category — lowest
   revenue but matches Technology's 17% profit margin

Business recommendations:
1. Reduce Furniture discounts immediately — current 17.4%
   average discount is unsustainable at a 2% margin
2. Invest more in Technology — best performing category
   on both revenue and profitability
3. Investigate Furniture cost structure — even with zero
   discounts the margins may still be concerning

What I learned:
High revenue does not equal high profit. Furniture
generates more revenue than Office Supplies but makes
6x less profit. Margin % tells the real story — not
total sales figures. This is one of the most important
lessons in business analytics.

### Task 5 — Two Way Lookup

Built a pricing matrix showing average unit price for
every Category × Region combination using AVERAGEIFS.

Matrix: 3 categories × 4 regions = 12 data points

Dynamic lookup formula:
=IFERROR(VLOOKUP(M6,K10:O13,
MATCH(M7,L10#,0)+1,0),"NotFound")

How it works:
- VLOOKUP finds the correct Category row
- MATCH finds the correct Region column number
- Together they return the exact cell intersection
- Dropdowns on Category and Region make it fully dynamic

Pricing matrix results:
Furniture       → $87–$94 across regions
Office Supplies → $31–$35 across regions
Technology      → $108–$135 across regions

Key findings:
1. Technology East is highest priced at $135.36 vs
   Central at $107.90 — $27.46 regional price gap
2. Furniture has most consistent pricing across regions
   — only $7.36 difference lowest to highest
3. Office Supplies South is highest at $34.52 despite
   South generally being a smaller revenue region

Business recommendation:
Investigate Technology pricing strategy — $27.46 gap
between East and Central for same products suggests
either different product mix per region or inconsistent
pricing policy that needs standardization.

What I learned:
VLOOKUP + MATCH together create a two dimensional lookup
system. VLOOKUP finds the row, MATCH finds the column.
This is the foundation of how real dashboards work —
every dropdown filter uses this same logic behind the scenes.

---

## 🔑 Key Formulas Learned
### Nested VLOOKUP
==VLOOKUP(VLOOKUP(Sheet1!A3,Order[[#All],[Order ID]:[Profit]],13,0),
Product[#All],{4,2,3},0)
Uses the result of one VLOOKUP as the lookup value
for another VLOOKUP.
Used for: pulling Product Name, Category, Sub-Category
from Products table using Order ID as starting point.

### VLOOKUP with IFERROR
=IFERROR(VLOOKUP([@[Product ID]],
Product[[#All],[Product ID]:[Sub-Category]],2,0),"MISSING")
Handles errors gracefully when lookup value is not found.
Used for: flagging orders with missing Product IDs.

### VLOOKUP + MATCH (two way lookup)
=IFERROR(VLOOKUP($M$6,$K$10:$O$13,
MATCH($M$7,$L$10#,0)+1,0),"NotFound")
VLOOKUP finds the correct row.
MATCH finds the correct column number dynamically.
Together they look up any cell in a 2D matrix.
Used for: dynamic pricing lookup by Category and Region.

---

## 😤 What Was Hard

### 1. Understanding nested VLOOKUP logic
Wrapping one VLOOKUP inside another was confusing at first.
The key that clicked was understanding that the inner
VLOOKUP runs first and its result becomes the lookup
value for the outer VLOOKUP. Once I understood the
order of execution it made complete sense.

### 2. VLOOKUP + MATCH two way lookup
Understanding why +1 was needed in the MATCH formula
was confusing. Had to think carefully about how VLOOKUP
counts columns from the first column of the matrix range
while MATCH counts only from the header row starting
at the first region column.

### 3. Realising Sales already includes Quantity
Almost multiplied Sales × Quantity for revenue which
would have doubled every figure. Had to carefully read
what the Sales column actually represented before
building any formula on top of it.

---

## 🔄 What I'd Do Differently

### 1. Plan my data model before splitting
I had to redo my Power Query split because Customer ID
ended up in the Products table. Next time I will sketch
out which columns belong in which table on paper before
touching Power Query. 2 minutes of planning saves
20 minutes of redoing work.

### 2. Build the nested VLOOKUP in stages
Instead of writing the full nested formula at once I
should build the inner VLOOKUP first, confirm it works,
then wrap it in the outer VLOOKUP. Building complex
formulas in stages makes them easier to debug.

### 3. Test formulas with edge cases immediately
After building each formula I should immediately test
with at least 3 different values including one that
should return an error to confirm IFERROR is working.
I only tested the happy path on some formulas today.

### 4. Label my Summary sheet sections clearly from the start
My Summary sheet got messy as I added more tasks.
Next time I will create labeled sections with headers
before building anything so each task has a designated
clean area from the beginning.
