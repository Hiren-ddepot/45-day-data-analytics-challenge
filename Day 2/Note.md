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


---

## 🔑 Key Formulas Learned


---

## 😤 What Was Hard


---

## 🔄 What I'd Do Differently
