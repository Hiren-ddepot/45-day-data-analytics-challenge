# Day 3 — VLOOKUP & Lookup Functions

**Date:** 31/03/2026
**Dataset:** Superstore Sales (9,994 rows, 21 columns)
**New dataset:** First day working with Superstore Sales
**Time spent:** [fill in at end]

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

---

## 🔑 Key Formulas Learned
[fill in at end]

---

## 😤 What Was Hard
[fill in at end]

---

## 🔄 What I'd Do Differently
[fill in at end]
