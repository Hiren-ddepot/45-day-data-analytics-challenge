# Day 9 — Advanced Functions
## Students Performance Dataset

**Date:** 16-04-2026
**Dataset A:** Students Performance (1,000 rows, 8 columns)
**Time spent:** 08:34:23

---

## 📋 Tasks Completed Today
Dataset A — Students Performance:
1. Dynamic Grade Lookup
2. Top 10 Per Subject
3. Test Prep Impact Analysis
4. Parental Education Impact
5. Failing Student Alert System

---

## ✅ Dataset A — Students Performance

### Pre-cleaning Step

Dataset had all text in lowercase on import.
Applied PROPER function to all text columns
before starting any analysis:  
=PROPER(A2)

Applied to:
- Gender
- Race/Ethnicity
- Parental Level of Education
- Lunch
- Test Preparation Course

Pasted as values and deleted original columns.
All text now properly capitalized. ✅

Column mapping confirmed:  
A — Gender  
B — Race/Ethnicity  
C — Parental Level of Education  
D — Lunch  
E — Test Preparation Course  
F — Math Score  
G — Reading Score  
H — Writing Score  

Score format verified:  
=ISNUMBER(F2) → TRUE ✅  
All scores stored as numbers not text.  
Score range: 0 to 100.

---

### Task 1 — Dynamic Grade Lookup

Built a grade boundary table on Summary sheet
sorted ascending (required for approximate match):


| Score   | Grade |
|--------|------|
| 0  |    F  |
| 60    |  D  |
| 70    |  C  |
| 80    |  B  |
| 90    |  A  |

Added 3 grade columns to working sheet using  
INDEX/MATCH with approximate match (1):  

Math Grade:   
=INDEX($T$3:$T$7,MATCH(G2,$S$3:$S$7,1))

Reading Grade:  
=INDEX($T$3:$T$7,MATCH(J2,$S$3:$S$7,1))

Writing Grade:  
=INDEX($T$3:$T$7,MATCH(M2,$S$3:$S$7,1))

How approximate match works:  
MATCH with 1 finds the largest boundary value
still less than or equal to the score.  
Score 75 → finds 70 → returns C  
Score 92 → finds 90 → returns A  
Score 55 → finds 0  → returns F  

Verified with 6 edge cases:  
Score 90 → A ✅  
Score 89 → B ✅  
Score 70 → C ✅  
Score 69 → D ✅  
Score 59 → F ✅  
Score 0  → F ✅  

Key lesson: Approximate match requires the
lookup table sorted ascending. If unsorted
MATCH returns wrong results silently —
no error just wrong answers. Always verify
with known edge cases before trusting results.

---

### Task 2 — Top 10 Per Subject

Built a Top 10 table on Summary sheet showing
highest scoring students per subject using
LARGE and INDEX/MATCH only — no sorting,
no pivot table used.

Student ID column added first since dataset
has no name column:  
      ="STU-"&TEXT(ROW()-1,"000")  
Gives STU-001 through STU-1000 as unique
identifiers for every student.

Tiebreaker column added to handle tied scores:  
=RANK(H2,$H$2:$H$1001,0)+COUNTIF($H$2:H2,H2)-1

How tiebreaker works:  
RANK gives base rank — tied students get
the same rank number.  
COUNTIF counts prior occurrences of same score.
Adding both gives every student a unique rank.

Example:
First student with score 100 → 1+0 = rank 1  
Second student with score 100 → 1+1 = rank 2  
Every student gets a unique rank number. ✅  

Top Score formula:  
=LARGE($H$2:$H$1001, A2)

Top Student ID formula:  
=INDEX(StudentsPerformance!$A$2:$A$1001,
MATCH(SUMMARY!A2,UniqueRankCol,0))

Applied separately to Math, Reading and Writing.

Top 3 results:  
Math:  
Rank 1 → Stu-150	→ 100    
Rank 2 → Stu-452	→ 100  
Rank 3 → Stu-459	→ 100

Reading:  
Rank 1 → Stu-107	→ 100  
Rank 2 → Stu-115	→ 100  
Rank 3 → Stu-150	→ 100  

Writing:  
Rank 1 → Stu-107	→ 100  
Rank 2 → Stu-115	→ 100  
Rank 3 → Stu-166	→ 100

Known limitation documented:  
MATCH returns first match for tied scores.  
Tiebreaker column resolves this by giving
every student a unique rank number based on
row position when scores are equal.  
In production a timestamp or secondary metric
would be a more meaningful tiebreaker than
row position.

---

## ✅ Task 3 — Test Prep Impact Analysis

Built comparison table using AVERAGEIF to
compare average scores for students who
completed test prep vs those who did not
across all 3 subjects.

Formulas used:

Math — Completed:
=AVERAGEIF(StudentsPerformance!F2:F1001,"Completed",StudentsPerformance!G2:G1001)

Math — None:
=AVERAGEIF(StudentsPerformance!F2:F1001,"None",StudentsPerformance!G2:G1001)

Math Lift %:
=(CompletedAvg-NoneAvg)/NoneAvg

Applied to Reading and Writing with same pattern.

Results:
| Subject  |  Completed  |  None  |  Lift % |
|----------|------------|---------|---------|
| Math    |   69.7    |     64.1  |  9%     |
| Reading |   73.9    |     66.5  |  11%    |
| Writing |   74.4    |     64.5  |  15%    |

Key findings:
1. Test prep improves scores across ALL subjects
2. Writing benefits most from test prep at +15%
3. Math benefits least at +9% but still significant
4. Reading improvement at +11% falls in the middle

Business insight: Test preparation has a
measurable positive impact on all three subjects.
Writing shows the highest improvement suggesting
writing skills are most teachable through
structured preparation.

Recommendation: Schools should prioritize
test prep enrollment especially for writing
where the impact is strongest. Students who
cannot access test prep are at a systematic
disadvantage of 9-15% across all subjects.

Socioeconomic note: If test prep access
correlates with income level this gap represents
a real equity issue worth investigating further.

---

## ✅ Task 4 — Parental Education Impact

Grouped students by parental level of education
(6 groups). Calculated average total score per
group using AVERAGEIF. Ranked groups using RANK.

Total Score column added first:
=F2+G2+H2

AVERAGEIF formula per education level:
=AVERAGEIF(Table1[Parental Level Of Education],"Master's Degree",Table1[Total Score])

Results ranked highest to lowest:
Rank    Education Level         Avg Total Score
1       Master's Degree         220.80
2       Bachelor's Degree       215.77
3       Associate's Degree      208.71
4       Some College            205.43
5       Some High School        195.32
6       High School             189.29

Key findings:
1. Clear positive correlation between parental
   education level and student performance
2. Master's Degree parents → highest avg 220.80
3. High School parents → lowest avg 189.29
4. Gap between highest and lowest = 31.51 points
5. Ranking follows education level almost perfectly
   except High School ranks below Some High School

Business insight: Parental education level
is a strong predictor of student performance.
Students whose parents have Master's degrees
score on average 31.5 points higher than
students whose parents only completed high school.

This reveals a systemic educational inequality —
students from less educated households need
additional support to close this gap.

Recommendation: Schools should identify students
from lower parental education backgrounds early
and provide targeted academic support, mentoring
and test preparation to help level the playing field.

---

## ✅ Task 5 — Failing Student Alert System

Identified at-risk students — defined as students
who scored below 60 (F grade) in 2 or more subjects.

Fail Count column formula:
=COUNTIF([@[Math Score]],"<60")
+COUNTIF([@[Reading Score]],"<60")
+COUNTIF([@[Writing Score]],"<60")

How it works:
Each COUNTIF returns 1 if score < 60 else 0.
Adding 3 COUNTIFs gives total fail count per student:
0 = no fails
1 = one subject failed
2 = two subjects failed (AT RISK)
3 = all three subjects failed (AT RISK)

At-Risk Flag column:
=IF([@[Fail Count]]>=2,"AT RISK","OK")

At-risk students identified:
=COUNTIF(FlagCol,"AT RISK") → 271 students

At-risk breakdown:
Safe students (0-1 fails):  729 (72.9%)
At-risk students (2-3 fails): 271 (27.1%)

Extracted at-risk list using FILTER:
=FILTER(Table1,Table1[Fail Count]>=2,"No at Risk Student")

Key finding: 271 students — 27.1% of the entire
cohort — are at risk of failing 2 or more subjects.
That is more than 1 in 4 students.

Business insight: A 27.1% at-risk rate is
significantly high and warrants immediate
intervention. These students need targeted
support before exams.

Smart formula note: Using COUNTIF on individual
cells (one cell range) returns 1 or 0 per cell.
Summing 3 of them counts total fails per student.
This is cleaner than nested IFs and more
scalable if additional subjects are added later.

---

## 💡 Key Formulas Learned Today

### INDEX/MATCH with approximate match
=INDEX(return_range, MATCH(value, lookup_range, 1))
Match type 1 = approximate match.
Finds largest value still ≤ lookup value.
Table MUST be sorted ascending.
Used for: grade boundary lookup.

### RANK + COUNTIF tiebreaker
=RANK(value,range,0)+COUNTIF($first:current,value)-1
Gives every row a unique rank even for tied values.
RANK gives base rank.
COUNTIF counts prior occurrences.
Used for: Top 10 ranking with tied scores.

### LARGE + INDEX/MATCH ranking
=INDEX(names, MATCH(LARGE(scores,N), scores, 0))
LARGE finds Nth highest value.
INDEX/MATCH finds who owns that value.
Used for: Top 10 students per subject.

### AVERAGEIF for group comparison
=AVERAGEIF(condition_range, condition, average_range)
Calculates average for rows meeting one condition.
Used for: test prep impact and parental education
analysis — comparing group averages.

### COUNTIF on single cell for fail counting
=COUNTIF([@[Score]],"<60")
Returns 1 if cell meets condition else 0.
Sum multiple COUNTIFs to count total fails per row.
Cleaner than nested IF for multi-subject fail count.
Used for: at-risk student identification.

### FILTER for extracting subsets
=FILTER(range, condition, if_empty)
Returns all rows meeting condition as spilled range.
Used for: extracting complete at-risk student list.

---

## 😤 What Was Hard

Figuring out the best formula to use for each task.

The failing student alert was a good example —
I had to think about whether to use nested IF,
IFS or COUNTIF to count fails per student.

The COUNTIF on a single cell approach was not
obvious at first. Most people would reach for
nested IF immediately. Taking time to think
about which tool fits best led to a cleaner
and more scalable solution.

---

## 🔄 What I'd Do Differently

Always understand the question correctly before
starting a task.

Every time I read the requirement fully and
planned my approach first the build was smooth.
When I jumped straight in I had to redo work.

Specific improvement: before writing any formula
I will now write the expected output in plain
English first. If I cannot describe what I want
in plain English I am not ready to write the formula.

Plain English first. Formula second. Always.

---

## 📁 Files in This Folder
- StudentsPerformance.xlsx
- Day 9 Advanced Functions - Students Performance
- notes.md

---

## 🔗 LinkedIn Post
[paste your LinkedIn post URL after posting]
