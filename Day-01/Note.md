# Day 1 — Formulas & Cell Referencing

**Date:** 28 March 2025
**Dataset:** IBM HR Analytics (1,470 employees, 3 departments)
**Time spent:** 02:41:12

---

## ✅ Tasks Completed

### Task 1 — Tiered Tax Calculator
Built a tax calculator using nested IFs across 4 income bands:
- 0–3,000 = 7%
- 3,001–7,000 = 15%
- 7,001–13,000 = 22%
- 13,001–19,999 = 30%

Calculated Tax Amount and Take Home Pay for all 1,470 employees.

Tax Formula used:
=IF(H2>13000,H2*30%,IF(H2>7000,H2*22%,IF(H2>3000,H2*15%,H2*7%)))

Take Home Formula used:
=H2-[TaxCell]

---

### Task 2 — Department Salary Audit
Built a summary table for all 3 departments showing:
Avg Salary, Min, Max, Total — for both Gross and Take Home Pay.
Added Tax Burden % column.

Formula used:
=AVERAGEIF(('Clean Table'!$E$2:$E$1471,'Clean Summary Table'!A15,'Clean Table'!$H$2:$H$1471)

=MINIFS('Clean Table'!$H$2:$H$1471,'Clean Table'!$E$2:$E$1471,'Clean Summary Table'!A15)

=MAXIFS('Clean Table'!$H$2:$H$1471,'Clean Table'!$E$2:$E$1471,'Clean Summary Table'!A15)

=SUMIF('Clean Table'!E3:E1472,'Clean Summary Table'!A16,'Clean Table'!H3:H1472)

Key finding:
Research & Development is highest paid Department - Gross: ($6,036,284.00), TakeHome ($4,774,674.43) while 
Human Resources Department is the lowest paid Department - Gross ($419,234.00), (TakeHome $327,536.43)

---

### Task 3 — Tenure Buckets
Dataset adjustment: No HireDate column available.
Used YearsAtCompany column directly (range: 0 to 40 years).

Buckets:
- 0–1 = New
- 2–5 = Developing
- 6–10 = Experienced
- 11+ = Veteran

Formula used:
=IF(B3<=29,"20-29",IF(B3<=39,"30-39",IF(B3<=49,"40-49","50+")))

What I learned: Real datasets don't always have the exact
column a task expects. A data analyst adapts and documents why.

---

### Task 4 — Employee ID Builder
Dataset adjustment: No HireDate column. Estimated hire year
using YEAR(TODAY()) - YearsAtCompany.

Format: JobRole(2) - Department(3) - EstHireYear - RowNumber
Example output: SA-SAL-2020-001

Formula used:
=LEFT(G4,2)&"-"&LEFT(E4,3)&"-"&YEAR(TODAY())-L4&"-"&TEXT(ROW()-1,"000")

Limitation: Estimated hire year may not be 100% accurate
since YearsAtCompany could be rounded. In a real project
I would flag this assumption to the stakeholder.

---

### Task 5 — Attrition Risk by Age Band
Created AgeBand column in Clean Summary Table using IFS on Age column.
Built summary table in Sheet 3 using COUNTIF and COUNTIFS.
Attrition Rate = employees who left / total per band.

Formulas used:
Total Employee per Age Band 
=COUNTIF('Clean Table'!$C$2:$C$1471,'Clean Summary Table'!A23)

Employees that Left
=COUNTIFS('Clean Table'!$C$2:$C$1471,'Clean Summary Table'!A23,'Clean Table'!$D$2:$D$1471,"Yes")

Key finding: Employees aged 20-29 have the highest attrition
rate. Employees aged 40-49 are the most stable age group.

Business insight: IBM should invest in retention strategies
for younger employees — mentorship programs, career growth
opportunities, and compensation reviews for entry-level roles.

Recommendation: HR should investigate exit interview data for
the 20-29 group to understand the root cause of departure.

---

## 💡 Key Formulas Learned Today
- Nested IF for tiered conditions
- AVERAGEIF, MINIFS, MAXIFS, SUMIF for conditional aggregation
- IFS for multiple condition bucketing
- COUNTIFS for multi-condition counting
- Text concatenation with & for ID building

---

## 😤 What Was Hard
Formatting Sheet 3 to look presentable took significant time.
Learned: good data presentation is a skill, not just an
afterthought. Will use Excel Table formatting going forward
to save time without sacrificing quality.

---

## 🔄 What I'd Do Differently

1. Structure first, build second
   I jumped into calculations before setting up my sheets
   properly. Next time I'll set up Sheet 1, 2 and 3 with
   headers and labels before touching any formula.

2. Format as I go, not at the end
   I left all formatting until the end and it cost me a lot
   of time. Going forward I'll format each table immediately
   after building it while the layout is still fresh in my mind.

3. Use Excel Table formatting from the start
   Insert → Table applies professional formatting in one click.
   I'll use this on every summary table from Day 2 onwards
   instead of manually formatting cells.

4. Document dataset adjustments immediately
   When I discovered there was no HireDate column I spent time
   figuring out the workaround. Next time I'll scan the dataset
   columns first before starting any task so I know exactly
   what I'm working with.

5. Read all 5 tasks before starting Task 1
   Some tasks build on each other. Reading all tasks first
   would help me plan my Sheet 2 columns better from the
   beginning instead of adding columns as I go.

---

## 📁 Files in This Folder
- Raw Data-HR-Employee-Attrition.xlsx
- HR-Employee-Attrition.xlsx
- IBM HR Analytics Task
- IBM HR Analytics Overview.png
- notes.md

---

## 🔗 LinkedIn Post
https://www.linkedin.com/posts/onyemazuwa-osonye_dataanalytics-excel-45daychallenge-ugcPost-7443737667537756160-W_vg?utm_source=share&utm_medium=member_desktop&rcm=ACoAAGUFJZsBzQiQhtf8rxqSkui47pnGPcohPMc
