# Day 4 — TS Academy Dashboard Assignment

**Date:** 01-04-2026
**Source:** TS Academy external assignment
**Dataset:** Nigerian Sales Data (2,098 transactions)
**Period:** January 2025 – May 2025
**Time spent:** 01:52:34

---

## 📋 Assignment Requirements
1. Data cleaning check
2. Calculate KPIs — Revenue, COGS, Profit, Customers
3. Pivot — Product by Profit
4. Pivot — Sales Rep by Revenue
5. Pivot — City by COGS
6. Pivot — Monthly Customer trend
7. Dashboard with all 4 charts
8. Region and Category slicers

---

## ✅ Data Cleaning
Dataset was clean — no missing values, no duplicates,
no inconsistent formatting found.

Columns already available:
- Total (Revenue = Unit Price × Quantity)
- Total Cost Price (COGS = Cost Price × Quantity)
- Profit (Total - Total Cost Price)

No additional cleaning required. Documented as baseline
for future reference.

---

## 📊 KPI Results
Total Revenue    → ₦2,328,370,000
Total COGS       → ₦1,862,696,000
Total Profit     → ₦465,674,000
Total Customers  → 2,098 transactions

Profit Margin    → 20% (Profit/Revenue)

---

## 🔑 Key Findings

### Product by Profit
Laptop A13        → ₦105.3M (most profitable)
Sofa Classic      → ₦69.2M
Desktop PC D21    → ₦68.6M
Smartphone Z10    → ₦52.0M
Refrigerator R55  → ₦48.3M
Air Conditioner   → ₦45.0M
Printer P50       → ₦38.9M
Office Chair Pro  → ₦18.1M
Microwave M20     → ₦13.4M
Blender B10       → ₦6.9M (least profitable)

### Sales Rep by Revenue
Peter     → ₦434.8M (top performer)
Musa      → ₦397.7M
David     → ₦378.7M
Aisha     → ₦378.0M
Chidinma  → ₦371.4M
Grace     → ₦367.8M

### City by COGS
Port Harcourt  → ₦493.8M (26%)
Lagos          → ₦486.8M (26%)
Abuja          → ₦458.8M (25%)
Kano           → ₦423.3M (23%)

### Monthly Customer Trend
January   → 520 customers
February  → 499 customers
March     → 525 customers
April     → 536 customers (peak)
May       → 18 customers (partial — 1 day only)

---

## 💡 Business Insights

1. Laptop A13 is the most profitable product at ₦105.3M —
   nearly double second place Sofa Classic at ₦69.2M.
   Recommendation: prioritize stock and promotion.

2. Peter is the top performing sales rep at ₦434.8M —
   9% ahead of second place Musa at ₦397.7M.
   Recommendation: study his approach and replicate.

3. April was the peak month with 536 customers — consistent
   growth from January to April shows positive business trend.
   Recommendation: plan promotions around March-April peak.

4. Port Harcourt and Lagos together account for 52% of total
   COGS. Recommendation: investigate operational costs in
   these cities to improve margins.

5. Blender B10 generates the lowest profit at ₦6.87M despite
   being a low cost product. Recommendation: review whether
   it is worth continuing to stock.

---

## 🔑 Key Formulas Used
- SUM for KPI totals
- Pivot Tables with calculated fields
- COUNTIF for customer count
- Grouped dates by month in pivot

---

## 😤 What Was Hard
Honestly nothing was technically difficult today.
The formulas and pivots came naturally from Days 1-3.

The only challenge was mental — planning the dashboard
layout before touching anything. I had to think about:
- Where to place 4 charts without it looking cluttered
- How to size the slicers alongside the charts
- Where the insights section would fit without
  scattering the design

Lesson: planning before building saves more time than
any formula shortcut. I spent 10 minutes sketching the
layout on paper before opening Excel and it made the
entire build smooth and fast.

---

## 🔄 What I'd Do Differently
1. I would sketch my dashboard layout on paper for
   every project going forward — not just Day 4.
   It saved significant time today.

2. I would add a profit margin % KPI card from the
   start. I only calculated it after finishing — it
   should have been in the original 4 KPI cards.
---

## 📁 Files in This Folder
- day04_ts_academy_dashboard.xlsx
- day04_ts_academy_dashboard.pdf
- notes.md
