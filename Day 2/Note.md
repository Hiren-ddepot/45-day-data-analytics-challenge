# Day 2 — Text & Date Functions

**Date:** 29-03-2026
**Dataset:** IBM HR Analytics (1,470 employees)
**Continuation of:** Day 1 — same dataset, new functions
**Time spent:** 

---

## 📋 Columns I'm working with today
- Age, Gender, MaritalStatus (text profile)
- JobRole, JobLevel, Department (seniority title)
- YearsAtCompany (review date estimation)
- MonthlyIncome, PercentSalaryHike, PerformanceRating (hike analysis)
- WorkLifeBalance, OverTime (burnout report)

---

## ✅ Tasks Completed
### Task 1 — Full Employee Profile Text Card

Columns used: EmployeeID, JobRole, Department, Age, 
MaritalStatus, YearsAtCompany

Dataset adjustment: No name column available. Used 
EmployeeID as identifier instead.

Formula used:
=A2&" is a "&G2&" in the "&E2&" department, aged "&B2&", "&LOWER(H2)&" and has been with the Company for "&Q2&"years"

Result: 1,470 unique employee profile sentences generated.

What I learned: LOWER() makes concatenated text read more
naturally in sentences. Small details like this matter when
presenting data to non-technical stakeholders.

#### Update: 
After completing Task 2, went back and replaced 
JobRole with SeniorityTitle in the profile text for a 
more descriptive and professional output

Updated formula:
=A2&" is a "&T2&" in the "&E2&" department, aged "&B2&", "&LOWER(I2)&" and has been with the Company for "&R2&"years"

### Task 2 — Seniority Title Builder

Formula used:
=IFS(G3=1,"Junior",G3=2,"Mid-Level",G3=3,"Senior",
G3=4,"Lead",G3=5,"Principal")&" "&TRIM(PROPER(H3))

Result: 1,470 employees assigned a proper seniority title
combining JobLevel and JobRole.
Example: Mid-Level Sales Executive

### Task 3 — Next Performance Review Date

Columns used: YearsAtCompany

Dataset adjustment: No HireDate column available.
Estimated join date by going back from today using EDATE.
Used YEAR(TODAY()) instead of hardcoding year so formula
stays accurate in future years.

Formulas used:

Estimated Join Date:
=EDATE(TODAY(),-YearsAtCompany*12)

Next Review Date:
=EDATE(EstimatedJoinDate,(INT(YearsAtCompany/2)+1)*24)

Review Flag:
=IF(NextReviewDate<=EDATE(TODAY(),12),"Due Soon","Upcoming")

Result: 1,470 employees assigned an estimated join date,
next review date, and review flag.

Key finding: 796 employees are Due Soon for a performance review.

What I learned:
- EDATE is cleaner than DATE for going backwards in time
- Using YEAR(TODAY()) instead of hardcoding 2026 makes
  formulas maintainable for future years
- Always think about whether a formula will still work
  in 2 years time 

PC note: PC went off mid task. Lesson learned — save with
Ctrl+S every 10 minutes and push to GitHub regularly so
no work is ever lost.

### Task 4 — Salary Hike Analysis

Columns used: PercentSalaryHike, MonthlyIncome, 
PerformanceRating

Formulas used:

New Salary after hike:
=MonthlyIncome*(1+PercentSalaryHike/100)

Annual Raise Amount:
=(NewSalary-MonthlyIncome)*12

Underpaid High Performer Flag:
=IF(AND(PercentSalaryHike<12,PerformanceRating=4),"Underpaid High Performer","Normal")

Key finding: Zero employees flagged as Underpaid High
Performers. Either all high performers received above
12% hike or no employee holds a PerformanceRating of 4.

Verification method: Manually edited one employee record
to PerformanceRating=4 and PercentSalaryHike=11 to test
the flag formula. Employee was correctly flagged as
"Underpaid High Performer" confirming the formula works.
Record reverted after testing.

Key finding: All 226 employees with PerformanceRating of 4
received 12% or above salary hike. IBM is correctly
rewarding high performers — zero underpaid high performers
in the dataset.

Business insight: IBM's compensation strategy appears to
be aligned with performance. No immediate action needed
for high performer retention from a salary perspective.

What I learned: Always test formulas by manually editing
a test case to confirm logic works correctly before
trusting the results. This is called unit testing.


---

## 🔑 Key Formulas Learned


---

## 😤 What Was Hard


---

## 🔄 What I'd Do Differently
