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

---

## 🔑 Key Formulas Learned
[fill in at end]

---

## 😤 What Was Hard
[fill in at end]

---

## 🔄 What I'd Do Differently
[fill in at end]
