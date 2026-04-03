# PART 1 — FOUNDATIONS

---

# Chapter 1 — Your First Real Formulas
## SUM, AVERAGE, COUNT and the Power of IF

### The Problem This Chapter Solves

Imagine you have a spreadsheet with 1,470 employee
salaries. Your manager asks:

- What is the total salary bill?
- What is the average salary?
- How many employees earn above ₦500,000?
- Which employees pay 30% tax vs 15% tax?

Without formulas you would manually add, divide
and check every single row. 1,470 times.

With the formulas in this chapter you answer all
four questions in under 2 minutes.

---

### Formula 1 — SUM

**What it does:**
Adds up a range of numbers. That is it.

**The syntax:**
=SUM(range)

**Real example:**
=SUM(E2:E1471)
Adds every value in column E from row 2 to row 1471.
Used to calculate total salary bill for 1,470 employees.

**What confused me:**
I kept typing =SUM(E2+E3+E4...) manually for each
cell. I did not know you could just select a range.
Once I learned E2:E1471 means "everything from E2
to E1471" it changed everything.

**The moment it clicked:**
The colon (:) means "to". E2:E1471 = E2 TO E1471.
Read it out loud that way every time until it sticks.

**Your turn:**
Download the IBM HR Analytics dataset from Kaggle.
Calculate the total MonthlyIncome for all employees.
Your answer should be around ₦6.5 million.

---

### Formula 2 — AVERAGE

**What it does:**
Calculates the mean (middle value) of a range.
Adds everything up then divides by the count.

**The syntax:**
=AVERAGE(range)

**Real example:**
=AVERAGE(E2:E1471)
Used to find the average monthly salary across
all 1,470 IBM employees.

**What confused me:**
I confused AVERAGE with MEDIAN. AVERAGE is the
mean — add everything and divide by count.
MEDIAN is the middle value when sorted.
They give different answers and mean different things.

**The moment it clicked:**
If 9 people earn ₦10,000 and 1 person earns
₦1,000,000 the AVERAGE is ₦109,000 — which
does not represent anyone accurately.
AVERAGE is pulled by extreme values.
Always ask yourself: is average the right measure here?

**Your turn:**
Calculate the average MonthlyIncome per department
using AVERAGEIF (covered later this chapter).
Which department has the highest average salary?

---

### Formula 3 — COUNT and COUNTA

**What it does:**
COUNT counts cells that contain numbers.
COUNTA counts cells that contain anything at all
(numbers, text, dates).

**The syntax:**
=COUNT(range)
=COUNTA(range)

**Real example:**
=COUNTA(A2:A1471)
Used to count how many employees are in the dataset.
Returns 1,470. ✅

**What confused me:**
I used COUNT on a column of names and got 0.
COUNT only counts numbers. Names are text.
Use COUNTA for text. Use COUNT for numbers.

**The moment it clicked:**
COUNT = Count numbers only
COUNTA = Count Anything (the A stands for All)

**Your turn:**
Count how many employees are in the Sales department
using COUNTIF (covered next).

---

### Formula 4 — IF

**What it does:**
Makes a decision. If something is true do this,
if it is false do that.

**The syntax:**
=IF(condition, value_if_true, value_if_false)

**Real example:**
=IF(E2>500000, "High Earner", "Standard")
If monthly salary is above 500,000 label as
High Earner, otherwise label as Standard.

**What confused me:**
The three parts confused me at first. I kept
forgetting the comma between each part and Excel
would throw an error.

Remember: IF has exactly 3 parts separated by 2 commas.
=IF( [is this true?] , [yes do this] , [no do this] )

**The moment it clicked:**
Think of IF as a question with two possible answers.
=IF( salary > 500000 , "High" , "Standard" )
Translation: "Is the salary above 500,000?
If yes say High. If no say Standard."

**Real world use:**
I used IF to build a tax calculator for 1,470
employees with 4 different tax bands:

=IF(E2>200000, E2*30%,
  IF(E2>100000, E2*22%,
    IF(E2>50000, E2*15%,
      E2*7%)))

This is called a nested IF — an IF inside an IF.
It checks each condition in order from top to bottom
and stops at the first one that is true.

**What confused me about nested IF:**
The closing brackets. Every IF needs one closing
bracket. Three nested IFs need three closing brackets
at the end. I kept getting bracket errors.

Tip: Count your opening brackets and make sure you
have the same number of closing brackets.

**The moment it clicked:**
Write it out in plain English first:
- If salary > 200,000 → 30% tax
- Else if salary > 100,000 → 22% tax
- Else if salary > 50,000 → 15% tax
- Otherwise → 7% tax

Then translate each line into the formula one at a time.

**Your turn:**
Build a tax calculator for the IBM HR dataset using
MonthlyIncome with these bands:
0 to 3,000 = 7%
3,001 to 7,000 = 15%
7,001 to 13,000 = 22%
13,001+ = 30%

---

### Formula 5 — AVERAGEIF, SUMIF, COUNTIF

**What they do:**
These are conditional versions of the basic formulas.
Instead of calculating for every cell they only
calculate for cells that meet a condition.

SUMIF = add only the values that meet a condition
AVERAGEIF = average only the values that meet a condition
COUNTIF = count only the cells that meet a condition

**The syntax:**
=SUMIF(range to check, condition, range to sum)
=AVERAGEIF(range to check, condition, range to average)
=COUNTIF(range to check, condition)

**Real example:**
=SUMIF(B2:B1471, "Sales", E2:E1471)
Add up all salaries (column E) but only for rows
where the Department (column B) is Sales.

=AVERAGEIF(B2:B1471, "Sales", E2:E1471)
Average salary for Sales department only.

=COUNTIF(B2:B1471, "Sales")
Count how many employees are in Sales.

**What confused me:**
The order of the arguments. Which range comes first?
Which comes last?

Remember this order:
1. WHERE to look (the condition column)
2. WHAT to look for (the condition value)
3. WHAT to calculate (the values column)

**The moment it clicked:**
Read it as a sentence:
=SUMIF(Department column, "Sales", Salary column)
"In the Department column, find Sales, then sum
the Salary column for those rows."

**Your turn:**
Build a department salary audit table using the
IBM HR dataset. For each of the 3 departments
calculate: Average salary, Min salary, Max salary,
Total salary.
Use AVERAGEIF, MINIFS, MAXIFS and SUMIF.

---

### Chapter 1 Summary

You now know the 5 most fundamental Excel formulas:

| Formula | What It Does |
|---------|--------------|
| SUM | Adds a range of numbers |
| AVERAGE | Calculates the mean of a range |
| COUNT/COUNTA | Counts cells with numbers/anything |
| IF | Makes a decision based on a condition |
| SUMIF/AVERAGEIF/COUNTIF | Conditional calculations |

These 5 formula families solve about 40% of all
real world Excel problems. Everything else builds
on top of them.

**Before moving to Chapter 2:**
Complete the Day 1 practice tasks using the IBM HR
Analytics dataset. Do not skip this. Reading about
formulas is not the same as building them yourself.

dataset: kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset 

---

### Coming Up in Chapter 2

Text and Date functions — how to clean messy data,
combine columns into sentences, calculate how long
someone has worked at a company, and build dynamic
dates that update automatically.

The dataset: IBM HR Analytics (same one).
The challenge: no HireDate column exists.
The solution: you will figure it out. I promise.
