# Day 7 — Data Cleaning
## Hotel Booking Demand Dataset

**Date:** 10/04/2026
**Dataset:** Hotel Booking Demand
**Source:** Kaggle — jessemostipak
**Time spent:** 

---

## 📋 Tasks Completed Today
1. Full Data Audit Report
2. Missing Value Strategy
3. Date Reconstruction
4. ADR Outlier Detection
5. Meal Code Standardizer

---

## ✅ Task 1 — Full Data Audit Report

Built a Data Audit sheet documenting every
issue found before touching any data.
This is what every professional analyst does
before cleaning — document first, clean second.


Raw dataset statistics:  
Total rows (inc header):   119,391  
Total rows (exc header):   119,390  
Total columns:             32  
Unique rows:               87,396  
Duplicate rows:            31,994 (26.8%)  
Blank cells:               0  
Inconsistent values:       0 visible  
NULL values found:  

| Column | NULL Count | % of Rows | Likely Meaning |
|--------|------------|-----------|----------------|
| children | 79,027 | 66.2% | No children on booking |
| company | 82,136 | 68.8% | Not a company booking |
| agent | 12,193 | 10.2% | No agent used |
| country | 452 | 0.4% | Country unknown |

Duplicate check process:  
Data → Remove Duplicates →  
All 32 columns selected →  
39,994 duplicate rows removed  
Remaining rows after dedup: 87,396 ✅   

Important note on duplicates:
In hotel bookings two guests can book the same
room type on the same date — that is not a
duplicate. Only rows that are 100% identical
across ALL 32 columns were removed.
This is why all 32 columns were selected before
running Remove Duplicates.

Audit findings summary:  
Issue 1: 31,994 duplicate rows (26.8%) — FIXED  
Issue 2: NULL values in 4 columns — see Task 2   
Issue 3: Date stored in 3 separate columns — see Task 3  
Issue 4: ADR column has outliers — see Task 4  
Issue 5: Meal codes are abbreviated — see Task 5    
Audit completed: 10/04/2026  

---

### Task 2 — Missing Value Strategy

Strategy: different fill values for different
column types based on business logic.  
Never fill blindly — always justify the choice.   

NULL replacement strategy:

Children → filled with 0  
Method: Ctrl+H Find & Replace NULL → 0  
Reason: NULL children means no children on
the booking. 0 is the correct numeric value.  
Count fixed: 79,027 cells updated.

Company → filled with 0  
Method: Ctrl+H Find & Replace NULL → 0  
Reason: NULL company means booking was not
made through a company. 0 = no company used.    
Count fixed: 82,136 cells updated.  

Agent → filled with 0   
Method: Ctrl+H Find & Replace NULL → 0  
Reason: NULL agent means no travel agent
was used. 0 = direct booking.  
Count fixed: 12,193 cells updated.  

Country → filled with "Unknown"  
Method: Ctrl+H Find & Replace NULL → Unknown  
Reason: Cannot assume a default country.  
Unknown is honest and prevents incorrect
geographic analysis.  
Count fixed: 452 cells updated.  

Verification after replacement:  
=COUNTIF(Children,"NULL")  → 0 ✅  
=COUNTIF(Company,"NULL")   → 0 ✅  
=COUNTIF(Agent,"NULL")     → 0 ✅  
=COUNTIF(Country,"NULL")   → 0 ✅  

Total NULL values fixed: 173,808 cells

Key lesson: The fill strategy depends on
what NULL actually means in context.  
For numeric columns where NULL = zero → use 0  
For categorical columns where NULL = unknown → use "Unknown"  
Never use the same fill value for every column
without thinking about what the data represents.
