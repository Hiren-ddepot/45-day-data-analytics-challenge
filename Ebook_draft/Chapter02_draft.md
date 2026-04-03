# Chapter 2 — Text and Date Functions
## Cleaning, Combining and Calculating with Text and Dates

### The Problem This Chapter Solves

Imagine you receive a dataset with 1,470 employees.
The names are in ALL CAPS. Some have extra spaces.
The hire dates are stored as text not real dates.
There is no full name column — just separate first
and last name fields.

Your manager wants:
- A clean employee ID combining department and year
- How long each employee has worked at the company
- When each employee is due for their next review
- A readable sentence describing each employee

Without text and date functions this takes hours
of manual editing. With the formulas in this chapter
it takes minutes.

---

### The Dataset We Are Using

IBM HR Analytics Dataset — 1,470 employees

Key columns we work with this chapter:
- Age
- Department
- JobRole
- JobLevel
- MaritalStatus
- YearsAtCompany
- Attrition
- WorkLifeBalance
- OverTime

Important note: This dataset has NO HireDate column.
That means we cannot use a date to calculate tenure.
Instead we use YearsAtCompany directly.

This is your first lesson in real world data analysis:
datasets never have exactly what you expect.
A good analyst adapts and documents why.

---

### Formula 1 — TRIM

**What it does:**
Removes extra spaces from text. Leading spaces,
trailing spaces and double spaces between words.
Leaves single spaces between words intact.

**The syntax:**
=TRIM(text)

**Real example:**
=TRIM(A2)
If A2 contains "  Sales Executive  " with extra
spaces TRIM returns "Sales Executive" — clean.

**What confused me:**
I thought TRIM removed ALL spaces including the
ones between words. It does not. It only removes
EXTRA spaces — spaces that should not be there.

**The moment it clicked:**
TRIM is like ironing a shirt. It smooths out the
wrinkles (extra spaces) but does not remove the
shirt itself (the spaces that belong there).

**Real world use:**
Always TRIM text columns before using them in
VLOOKUP or XLOOKUP. A lookup for "Sales" will
fail to find " Sales" (with a leading space) even
though they look identical on screen.

**Your turn:**
In the IBM HR dataset apply TRIM to the Department
column in a helper column. Check if any departments
had hidden spaces causing COUNTIF to miss them.

---

### Formula 2 — PROPER, UPPER, LOWER

**What they do:**
PROPER → Capitalizes First Letter Of Each Word
UPPER → CONVERTS EVERYTHING TO UPPERCASE
LOWER → converts everything to lowercase

**The syntax:**
=PROPER(text)
=UPPER(text)
=LOWER(text)

**Real example:**
=PROPER(A2)
If A2 contains "sales executive" PROPER returns
"Sales Executive" — professionally formatted.

=LOWER(C2)
If C2 contains "Married" LOWER returns "married"
Used to make text read naturally inside sentences:
"John is a Senior Manager, married, with 6 years
at the company."

**What confused me:**
PROPER capitalizes the first letter of EVERY word
including small words like "of" and "the". So
"head of department" becomes "Head Of Department"
which is not always correct grammar.

**The moment it clicked:**
Use PROPER for names and job titles where every
word should be capitalized. Use LOWER for words
inside sentences where only the first word needs
a capital letter.

**Your turn:**
Apply PROPER to the JobRole column in a helper
column. Then combine it with the Department column
to create a formatted job title like:
"Sales Executive — Sales Department"

---

### Formula 3 — CONCATENATE and the & operator

**What it does:**
Combines multiple text values into one cell.
CONCATENATE and & do the same thing.
& is faster to type and more commonly used.

**The syntax:**
=CONCATENATE(text1, text2, text3)
=text1 & text2 & text3

**Real example:**
="EMP"&"-"&B2&"-"&YEAR(HireDate)&"-"&TEXT(ROW()-1,"000")
Used to build a unique Employee ID like EMP-SAL-2018-001

Since our dataset has no HireDate we adapted:
="EMP"&"-"&LEFT(B2,3)&"-"&(DATE(YEAR(TODAY))-D2)&"-"&TEXT(ROW()-1,"000")
Where B2 = Department and D2 = YearsAtCompany

**What confused me:**
Forgetting to add spaces and separators between
the combined values. "JohnSmith" instead of "John Smith"

The fix: add " " (space in quotes) between values:
=A2&" "&B2
This gives "John Smith" not "JohnSmith"

**The moment it clicked:**
The & works like glue. Whatever you put between
the & symbols gets stuck together in order.
Add your separators (spaces, dashes, commas) as
text in quotes between each piece.

**Real world use:**
I built a full employee profile sentence combining
6 columns into one readable description:

=K2&" is a "&L2&" in the "&B2&" department, aged
"&A2&", "&LOWER(E2)&", and has been with the
company for "&D2&" years."

Output:
"EMP-SAL-2018-001 is a Senior Sales Executive in
the Sales department, aged 35, married, and has
been with the company for 6 years."

**Your turn:**
Build an employee profile sentence for every row
in the IBM HR dataset using these columns:
EmployeeID, SeniorityTitle, Department, Age,
MaritalStatus, YearsAtCompany

---

### Formula 4 — LEFT, RIGHT, MID

**What they do:**
Extract a specific number of characters from text.

LEFT → extracts from the beginning
RIGHT → extracts from the end
MID → extracts from the middle

**The syntax:**
=LEFT(text, number of characters)
=RIGHT(text, number of characters)
=MID(text, start position, number of characters)

**Real example:**
=LEFT(B2,3)
If B2 = "Sales" LEFT returns "Sal"
Used to extract the first 3 letters of Department
for building Employee IDs.

=MID("01/01/2018",7,4)
Extracts "2018" — the year from a date stored as text.
Start position 7, extract 4 characters.

**What confused me:**
MID needs 3 arguments not 2. I kept forgetting the
start position and only giving it text and length.

Remember:
LEFT needs 2 things — text and how many from left
RIGHT needs 2 things — text and how many from right
MID needs 3 things — text, where to start, how many

**The moment it clicked:**
Think of your text as a row of numbered seats.
LEFT starts at seat 1.
RIGHT starts at the last seat and counts backwards.
MID lets you pick any seat to start from.

**Your turn:**
Extract the first 2 letters of JobRole and the
first 3 letters of Department. Combine them with
YearsAtCompany to build a unique Employee ID.

---

### Formula 5 — IFS for text bucketing

**What it does:**
Assigns a text label based on multiple conditions.
Cleaner than nested IF for 3 or more conditions.

**The syntax:**
=IFS(condition1, result1, condition2, result2, ...)

**Real example:**
=IFS(D2<=1,"New",D2<=5,"Developing",
D2<=10,"Experienced",D2>10,"Veteran")

Where D2 = YearsAtCompany.
Assigns a tenure label to every employee.

Dataset adjustment: No HireDate column existed so
we used YearsAtCompany directly instead of
calculating from a hire date. This is a common
real world adaptation.

**What confused me:**
IFS checks conditions IN ORDER and stops at the
first true condition. So the order matters.

If you write:
=IFS(D2>0,"New", D2>5,"Developing", D2>10,"Veteran")

Every employee gets "New" because D2>0 is always
true first. You must order from most restrictive
to least restrictive — or from smallest to largest.

**The moment it clicked:**
IFS is like a series of questions asked in order.
The moment one answer is YES it stops asking.
So put your most specific questions first.

**Your turn:**
Create age bands for the IBM HR dataset using IFS:
20-29 = "Early Career"
30-39 = "Mid Career"
40-49 = "Senior Career"
50+ = "Late Career"

Then use COUNTIFS to find the attrition rate
per age band. Which age group leaves most often?

---

### Formula 6 — EDATE

**What it does:**
Adds or subtracts a number of months from a date.
Returns a new date.

**The syntax:**
=EDATE(start_date, months)

Positive months = future date
Negative months = past date

**Real example:**
Estimated join date (going backwards from today):
=EDATE(TODAY(), -YearsAtCompany*12)

This goes back YearsAtCompany years from today
by converting years to months (×12).

Next performance review date (going forwards):
=EDATE(EstimatedJoinDate,(INT(YearsAtCompany/2)+1)*24)

Reviews happen every 2 years. This finds the next
upcoming review by calculating how many 2-year
cycles have passed then adding one more.

**What confused me:**
EDATE works in MONTHS not years. To go back 6 years
you need to use -72 (6 × 12) not -6.

**The moment it clicked:**
EDATE only speaks months. Convert everything to
months before giving it to EDATE.
Years × 12 = months. Always.

**Important improvement I made:**
Instead of hardcoding the current year as 2026 I used:
=YEAR(TODAY())

This means the formula automatically updates every
year without needing to manually change 2026 to 2027.
That is called making a formula maintainable.
Always think about whether your formula will still
work in 2 years.

**Your turn:**
Calculate the estimated join date for every employee
using EDATE and YearsAtCompany.
Then calculate their next performance review date
assuming reviews happen every 2 years.
Flag employees whose next review is within 12 months
as "Due Soon" using an IF formula.

---

### Formula 7 — DATEDIF

**What it does:**
Calculates the difference between two dates in
days, months or years.

**The syntax:**
=DATEDIF(start_date, end_date, unit)

Units:
"Y" = complete years
"M" = complete months
"D" = days

**Real example:**
=DATEDIF(HireDate, TODAY(), "Y")
Calculates exact years of service.

Since our dataset has no HireDate we used:
YearsAtCompany column directly — which is the
same result already calculated for us.

**What confused me:**
DATEDIF is a hidden function — it does not appear
in Excel autocomplete. You have to type it exactly.
Many people do not even know it exists.

**The moment it clicked:**
DATEDIF = DATE DIFference. The arguments go in
chronological order: start date first, end date second.
Getting them backwards gives you a negative number
or an error.

**Your turn:**
If you have a dataset with actual hire dates use
DATEDIF to calculate exact tenure in years.
Compare the result with any YearsAtCompany column
that exists — are they the same?

---

### Real Project — Employee Profile System

Combining everything from this chapter here is
the complete employee profile system built on the
IBM HR Analytics dataset:

**Sheet 2 — Working calculations**

Column added:    Formula used:
TenureBucket    =IFS(D2<=1,"New",D2<=5,"Developing",
                D2<=10,"Experienced",D2>10,"Veteran")

SeniorityTitle  =IFS(G2=1,"Junior",G2=2,"Mid-Level",
                G2=3,"Senior",G2=4,"Lead",
                G2=5,"Principal")&" "&TRIM(PROPER(H2))

EstJoinDate     =EDATE(TODAY(),-D2*12)

NextReview      =EDATE(P2,(INT(D2/2)+1)*24)

ReviewFlag      =IF(Q2<=EDATE(TODAY(),12),
                "Due Soon","Upcoming")

EmployeeID      =LEFT(C2,2)&"-"&LEFT(B2,3)&"-"&
                (YEAR(TODAY())-D2)&"-"&TEXT(ROW()-1,"000")

ProfileSentence =K2&" is a "&J2&" in the "&B2&
                " department, aged "&A2&", "&
                LOWER(E2)&", and has been with
                the company for "&D2&" years."

**Sheet 3 — Summary outputs**

WLB vs Overtime table:
WLB Level    Total    On Overtime    Rate
Poor         80       22             28%
Fair         344      104            30%
Good         893      254            28%
Excellent    153      36             24%

Key finding: Fair WLB group has the highest overtime
rate at 30% — not Poor as expected. This tells us
overtime alone does not drive poor work life balance.
Other factors like stress and management style
likely play a bigger role.

---

### Chapter 2 Summary

| Formula | What It Does |
|---------|--------------|
| TRIM | Removes extra spaces from text |
| PROPER/UPPER/LOWER | Changes text capitalization |
| & and CONCATENATE | Combines text values together |
| LEFT/RIGHT/MID | Extracts characters from text |
| IFS | Labels based on multiple conditions |
| EDATE | Adds or subtracts months from a date |
| DATEDIF | Calculates difference between two dates |

**The most important lesson from this chapter:**

Real datasets never have exactly what you need.
The IBM HR dataset had no HireDate column.
No name column. No calculated tenure.

A beginner panics and gives up.
An analyst adapts, documents the adjustment,
and builds the solution with what is available.

That is the difference.

---

### Coming Up in Chapter 3

Lookup functions — VLOOKUP, XLOOKUP and nested
lookups across multiple sheets.

The dataset: Superstore Sales (9,994 orders)
The challenge: Product data lives in a separate
sheet from order data.
The solution: nested XLOOKUP connecting two sheets
with one formula.

Spoiler: you already know how to do part of it
from this chapter. The & operator and LEFT formula
you just learned will appear again in a new context.
