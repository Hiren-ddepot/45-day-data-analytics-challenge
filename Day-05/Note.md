# Day 5 — XLOOKUP, IF Logic & SUMIFS
## Superstore Sales Dataset

**Date:** April 2, 2026
**Dataset:** Superstore Sales (9,994 rows, 21 columns)
**Continuation of:** Day 3 — same dataset, deeper analysis
**Time spent:** [fill in at end]

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



---

## 💡 Key Formulas Learned Today

### XLOOKUP with search_mode=-1
=XLOOKUP(lookup, lookup_array, return_array,
"Not Found", 0, -1)
Searches from last row upward — always returns
most recent record. Essential for time-based lookups.

### Nested XLOOKUP across sheets
=XLOOKUP(XLOOKUP(B1,Sheet1[Col],Sheet1[Col2]),
Sheet2[Col],Sheet2[Col2])
Inner XLOOKUP result feeds outer XLOOKUP.
Applied independently from Day 3 nested VLOOKUP concept.


---

## 😤 What Was Hard
[fill in your honest struggle]

---

## 🔄 What I'd Do Differently
[fill in your honest reflection]

---

## 📁 Files in This Folder
- day04_superstore.xlsx
- notes.md

---

## 🔗 LinkedIn Post
[paste your LinkedIn post URL after posting]
