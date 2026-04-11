# Day 8 — Data Cleaning
## Hotel Booking Demand Dataset

**Date:** 10/04/2026
**Dataset:** Hotel Booking Demand
**Source:** Kaggle — jessemostipak
**Time spent: 03:11:25

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
| company | 82,137 | 68.8% | Not a company booking |
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

---

## ✅ Task 3 — Date Reconstruction

Problem: Arrival date stored in 3 separate columns:  
- arrival_date_year (number)  
- arrival_date_month (text e.g. "July")  
- arrival_date_day_of_month (number)  

Solution: Combined into one real Excel date
using DATE + MATCH formula.

Formula used:  
=DATE(
[@arrival_date_year],
MATCH([@arrival_date_month],
{"January","February","March","April","May",
"June","July","August","September","October",
"November","December"},0),
[@arrival_date_day_of_month]
)

How it works:  
DATE needs Year, Month Number and Day.  
Year and Day were already numbers.  
Month was text so MATCH converted it to a
number by finding its position in the hardcoded
month list. January=1, February=2...December=12.

New column added: Arrival Date  
Formatted as: DD/MM/YYYY

Verification:  
Formula returned correct dates ✅  
Dates sorted chronologically ✅  
Earliest arrival date: 7/27/2015  
Latest arrival date: 9/4/2017  

Key lesson: Real datasets rarely store dates
in a single usable column. Learning to reconstruct
dates from separate parts is an essential
data cleaning skill used in almost every
real world project.

Dataset adjustment note:  
Used MATCH with hardcoded month array instead
of a lookup table — more portable and does not
require a separate reference sheet.

---

## ✅ Task 4 — ADR Outlier Detection

ADR = Average Daily Rate (price per night)

Used IQR (Interquartile Range) method to
identify statistical outliers in the ADR column.

Calculations:  
Q1  = QUARTILE(ADR, 1) = 72  
Q3  = QUARTILE(ADR, 3) = 134  
IQR = Q3 - Q1 = 62  

Outlier boundaries:  
Lower bound = Q1 - 1.5×IQR = 72 - 93 = -21  
Upper bound = Q3 + 1.5×IQR = 134 + 93 = 227  

Additional rule: ADR = 0 forced to OUTLIER
because a hotel booking with zero daily rate
is either a data error or a complimentary
booking that should not skew rate analysis.

Flag formula:  
=IF(OR([@Adr]=0,
[@Adr]<'Data Audit'!$C$22-1.5*'Data Audit'!$C$24,
[@Adr]>'Data Audit'!$C$23+1.5*'Data Audit'!C24),
"OUTLIER","OK")

Formula references Q1, Q3 and IQR stored on
Data Audit sheet — centralizing the boundary
values so changing Q1/Q3 updates all flags
automatically without editing the formula.

Results:
OUTLIER count: =COUNTIF(FlagCol,"OUTLIER")  
 23,595    
OK count: =COUNTIF(FlagCol,"OK")  
63,801  

Key decision: Forcing ADR=0 to OUTLIER was
a deliberate business logic choice beyond the
standard IQR method. Zero rate bookings distort
average rate calculations and should be
investigated separately — they may represent
staff bookings, errors, or complimentary stays.

This is a real world analyst judgment call —
statistical methods give you a starting point
but domain knowledge refines the rules.

Key lesson: IQR method flags values that are
statistically extreme. But statistics alone
do not determine outlier rules — business
context always matters. A good analyst combines
both to make defensible decisions.

---

## ✅ Task 5 — Meal Code Standardizer

Problem: Meal column contains abbreviated codes
that are meaningless to non-technical readers:
BB, HB, FB, SC, Undefined

Solution: Created a standardized meal description
column using SWITCH formula then locked the
original column with data validation to prevent
future bad entries.

SWITCH formula:
=SWITCH([@Meal],
"BB","Bed & Breakfast",
"HB","Half Board",
"FB","Full Board",
"SC","Self Catering",
"Unknown")

How SWITCH works:
Checks [@Meal] against each code in order.
When a match is found it returns the full name.
Last argument "Unknown" is the default — returned
when no code matches (handles Undefined entries).

Data validation applied to original Meal column:
Data → Data Validation → List →
Source: BB,HB,FB,SC,Undefined
This prevents future entries outside the valid list.

Meal code distribution:  
 Bed & Breakfast - 67,978  
 Full Board - 360   
 Half Board - 9,085   
 Self Catering - 9,481   
 Unknown 	 492   

Key lesson: Data standardization serves two purposes:
1. Makes current data readable for non-technical
   stakeholders who do not know what BB or HB means
2. Prevents future data quality issues by locking
   down valid input values with data validation

A cleaned dataset without prevention measures
will become dirty again. Always add validation
after cleaning.

---

## 💡 Key Skills Learned Today

### Find & Replace for bulk NULL fixing
Ctrl+H → Find NULL → Replace with value
Fastest way to fix thousands of NULL cells
across an entire column simultaneously.

### DATE + MATCH for date reconstruction
=DATE(year, MATCH(month_name, month_array, 0), day)
Converts text month names to numbers using MATCH
then combines with year and day into real Excel date.
Works even when month is stored as text.

### IQR outlier detection method
Q1 = QUARTILE(col, 1)
Q3 = QUARTILE(col, 3)
IQR = Q3 - Q1
Lower = Q1 - 1.5×IQR
Upper = Q3 + 1.5×IQR
Flag anything outside these bounds as outlier.
Standard statistical method used in real analytics.

### Forcing business logic outliers
=IF(OR(value=0, value<lower, value>upper),
"OUTLIER","OK")
Adding OR(value=0) forces zero values to be
flagged regardless of IQR bounds.
Combines statistical method with business judgment.

### SWITCH for code standardization
=SWITCH(value, code1, label1, code2, label2, default)
Cleaner than nested IF for code-to-label mapping.
Last argument = default for unmatched values.

### Centralizing calculation inputs
Storing Q1, Q3, IQR on a reference sheet and
using absolute references ($C$22) in formulas.
Changing the reference value updates all
dependent formulas automatically.
This is called a single source of truth — a
professional practice that prevents inconsistency.

### Data validation as prevention
After cleaning apply data validation to prevent
the same issues recurring.
Cleaning fixes the past.
Validation protects the future.

---

## 😤 What Was Hard

Even though the tasks were technically straightforward, a few areas required careful thinking and reinforced important data analysis principles:  
### 1. Understanding What NULL Actually Means  
    The biggest “challenge” was not technical, but conceptual.  
    NULL values appeared in multiple columns, but each had a different meaning:  
    - In children, NULL meant 0  
    - In company and agent, NULL meant not applicable  
    - In country, NULL meant missing information  

This required slowing down and thinking in terms of business context, not just applying one generic fix.

👉 Key takeaway:  
Data cleaning is not about tools — it’s about understanding what the data represents.

### 2. Distinguishing True vs False Duplicates
    At first glance, 31,994 duplicate rows looked like an obvious issue.  
    However, in a hotel dataset:  
    Multiple bookings can share similar attributes but are not necessarily duplicates  
    The challenge was ensuring that only 100% identical rows across all 32 columns were removed.  

👉 Key takeaway:  
Not all duplicates are errors — context determines correctness.  

### 3. Applying Statistical vs Business Logic (Outliers)
    The IQR method provided clear statistical boundaries, but it did not automatically classify ADR = 0 as an outlier.  
    This required an additional decision:  
    Statistically valid? → Maybe  
    Business meaningful? → No  

👉 Key takeaway:
Real-world analysis requires combining:  
  Statistical methods  
  Domain judgment

---

## 🔄 What I'd Do Differently
If I were to redo this project, I would improve it in the following ways:

### 1. Automate the Cleaning Process
Most steps were done manually (Find & Replace, formulas, etc.).  
Next time, I would:  
    Use Power Query to automate cleaning steps  
    Create a repeatable pipeline

👉 This would make the process scalable and reusable.

### 2. Add Data Validation Earlier
Data validation was applied only after cleaning.  
In future:  
    I would design validation rules before or during cleaning
    This would reduce the need for corrections later

### 3. Build a Data Dictionary
While cleaning, I inferred meanings of columns (e.g., NULL behavior).  
Next time:  
    I would create a data dictionary upfront  
    Define each column clearly before cleaning  

👉 This improves clarity and prevents assumptions.

### 4. Visualize Issues Earlier
Most issues were identified using formulas and counts.  
Next time, I would:  
    Use charts (box plots, distributions) earlier
    Visually detect outliers and anomalies faster

---

## 📁 Files in This Folder
- day07_hotel_booking_clean.xlsx
- day07_hotel_booking_raw.xlsx (original backup)
- Hotel Data Audit.png
- notes.md

---

## 🔗 LinkedIn Post
