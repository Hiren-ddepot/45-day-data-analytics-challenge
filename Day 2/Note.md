# Day 2 — Text & Date Functions

**Date:** 29-03-2026
**Dataset:** IBM HR Analytics (1,470 employees)
**Continuation of:** Day 1 — same dataset, new functions
**Time spent:** 03:15:28

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

### Task 5 — Work Life Balance & Overtime Report

Columns used: WorkLifeBalance, OverTime

WLB Label formula:
=IFS(D2=1,"Poor",D2=2,"Fair",D2=3,"Good",D2=4,"Excellent")

Overtime Rate formula:
=COUNTIFS(WorkLifeBalanceCol,"Poor",OvertimeCol,"Yes")/COUNTIF(WorkLifeBalanceCol,"Poor")

Results:
Poor      → 80 employees  | 22 on OT  | 28%
Fair      → 344 employees | 104 on OT | 30%
Good      → 893 employees | 254 on OT | 28%
Excellent → 153 employees | 36 on OT  | 24%

Key finding: Fair WorkLifeBalance group has the highest overtime rate
at 30%. Excellent WorkLifeBalance group has the lowest at 24%.
Surprisingly Poor WorkLifeBalance does not have the highest overtime
rate suggesting overtime is not the sole driver of poor
work life balance.

Business insight: IBM should investigate other factors
beyond overtime that contribute to poor work life balance
such as management quality, job demands, and stress levels.
Reducing overtime alone will not solve the WLB problem.

What I learned: Data does not always confirm what you
expect. Surprising results are often the most valuable
insights — they challenge assumptions and lead to deeper
investigation.


---

## 🔑 Key Formulas Learned

### EDATE
=EDATE(TODAY(), -YearsAtCompany*12)
Goes forward or backward by a number of months from a date.
Used for: estimating join date and calculating next review date.

### IFS for text labels
=IFS(D2=1,"Poor",D2=2,"Fair",D2=3,"Good",D2=4,"Excellent")
Assigns a text label based on a numeric code.
Used for: WLB labels and Seniority titles.

### EDATE for future dates
=EDATE(EstimatedJoinDate,(INT(YearsAtCompany/2)+1)*24)
Calculates a future date by adding months to a base date.
Used for: next performance review date.

### IF with AND
=IF(AND(O2<12,S2=4),"Underpaid High Performer","Normal")
Checks multiple conditions simultaneously before flagging.
Used for: identifying underpaid high performers.

### COUNTIFS for summary tables
=COUNTIFS(WLBCol,"Poor",OvertimeCol,"Yes")
Counts rows that meet multiple conditions at once.
Used for: overtime count per WLB group.

### Text concatenation with &
=EmployeeID&" is a "&SeniorityTitle&" in the "&Department
Combines multiple columns into one readable sentence.
Used for: employee profile text card.

### TRIM and PROPER
=TRIM(PROPER(JobRole))
PROPER capitalizes first letter of each word.
TRIM removes extra spaces before and after text.
Used for: cleaning JobRole text in seniority title.

### LOWER
=LOWER(MaritalStatus)
Converts text to lowercase for natural reading in sentences.
Used for: making marital status read naturally in profile card.

### YEAR(TODAY())
=YEAR(TODAY())
Returns the current year dynamically.
Used for: making date formulas work in any future year
without hardcoding 2024.


---

## 😤 What Was Hard

### 1. EDATE logic for Next Review Date
Understanding how to combine EDATE with INT and YearsAtCompany
to calculate a future review date was confusing at first.
The math of (INT(YearsAtCompany/2)+1)*24 took time to
understand — it finds how many 2-year cycles have passed
then adds one more cycle to get the next upcoming review.

---

## 🔄 What I'd Do Differently

### 1. Save and push to GitHub more frequently
My PC went off mid task and I almost lost my work.
Going forward I will save with Ctrl+S every 10 minutes
and push to GitHub after completing each task — not just
at the end of the day.

### 2. Check column references before using full column
I wasted time investigating why COUNTIF(S:S,4) returned 0.
Next time I will use exact range references from the start
e.g. S2:S1471 instead of S:S to avoid data type issues.

### 3. Scan all dataset columns before starting
I would spend 5 minutes at the beginning reading through
all column names and understanding what each one contains
before writing any formula. This prevents surprises mid task
like discovering a column contains text instead of numbers.

### 4. Plan Sheet 2 columns in advance
I kept adding columns as I went which made the sheet
disorganised. Next time I will map out all the columns
I need for the day before building anything so Sheet 2
stays clean and structured from the start.

### 5. Question my assumptions before interpreting results
I assumed Poor WLB would have the highest overtime rate
but the data said otherwise. Going forward I will let
the data tell the story rather than expecting a specific
outcome before running the analysis.
