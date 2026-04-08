# Chapter 3 — Lookup Functions
## VLOOKUP, XLOOKUP and Connecting Data Across Sheets

### The Problem This Chapter Solves

Imagine you have two separate spreadsheets.

One has 9,994 customer orders — Order ID, Customer,
Product ID, Quantity, Sales, Profit.

The other has your product catalog — Product ID,
Product Name, Category, Sub-Category.

Your manager wants a report showing every order
with the full product name and category included.

Manually copying and pasting would take hours.
One lookup formula does it in seconds.

That is what this chapter is about.

---

### The Dataset We Are Using

Superstore Sales Dataset — 9,994 orders

We split it into two tables:
- Orders table: transaction data per order
- Products table: product master data

This simulates how real business data is structured.
In real companies orders and products are ALWAYS
in separate tables — sometimes even separate systems.
Learning to connect them is a core analyst skill.

---

### Before We Start — Understanding Keys

Every lookup formula needs a KEY.

A key is a value that exists in BOTH tables and
uniquely identifies each row.

In our case:
- Orders table has Product ID
- Products table has Product ID
- Product ID is our KEY

| Orders table | Products table |
|--------------|----------------|
| Order ID | Product ID  ← KEY |
| Customer ID | Product Name |
| Product ID  ← KEY | Category |
| Quantity | Sub-Category |
| Sales | |

When you look something up you are saying:
"Find this key in that table and bring back
the value from the column I specify."

This concept — connecting tables through a shared
key — is the foundation of every database system
in the world. SQL, Power BI, Python pandas — they
all use the same idea. Learn it well here in Excel
and it transfers everywhere.

---

### Formula 1 — VLOOKUP

**What it does:**
Looks up a value in the first column of a table
and returns a value from a specified column
in the same row.

V = Vertical. It searches DOWN a column.

**The syntax:**
=VLOOKUP(what to find, where to look, which column, exact?)

4 parts. Always 4 parts.

**Breaking down each part:**

Part 1 — what to find:
The key value you are searching for.
Usually a cell reference like A2.

Part 2 — where to look:
The entire table including the key column.
Must start with the key column on the LEFT.
Lock it with $ signs so it does not shift
when you drag the formula down.

Part 3 — which column:
A number telling VLOOKUP which column to return.
1 = first column (the key itself)
2 = second column
3 = third column
And so on.

Part 4 — exact match:
0 or FALSE = exact match (use this almost always)
1 or TRUE = approximate match (for number ranges)

**Real example:**
=VLOOKUP(B2, Products!$A:$D, 2, 0)

Translation:
- Find the value in B2 (Product ID)
- Search in Products sheet columns A to D
- Return column 2 (Product Name)
- Exact match (0)

**What confused me:**
The column number counts from the START of your
table range — not from column A of the spreadsheet.

If your table is D:G and you want column F
that is column 3 in the range — not column F.

**The moment it clicked:**
The column number is relative to where you told
VLOOKUP to look — not where it is on the sheet.
Always count from the left edge of your range.

---

### The Biggest VLOOKUP Mistake

VLOOKUP can only look to the RIGHT.

The key column MUST be the leftmost column
in your table range. If your key is in column C
and you need data from column A — VLOOKUP cannot
do it. You need INDEX+MATCH or XLOOKUP instead.

This is why many analysts have moved away from
VLOOKUP entirely. XLOOKUP has no such restriction.

---

### VLOOKUP Approximate Match — Tiered Lookups

Most of the time you want exact match (0).
But approximate match (1) is powerful for
finding which tier or band a value falls into.

**Real example — tiered discount engine:**

Build a discount table sorted ascending:
