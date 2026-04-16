# Day 9 — Advanced Functions
## Students Performance Dataset

**Date:** 16-04-2026
**Dataset A:** Students Performance (1,000 rows, 8 columns)
**Dataset B:** FIFA Player Ratings (to be completed)
**Time spent:** [fill in at end]

---

## 📋 Tasks Completed Today
Dataset A — Students Performance:
1. Dynamic Grade Lookup
2. Top 10 Per Subject
3. Test Prep Impact Analysis
4. Parental Education Impact
5. Failing Student Alert System

Dataset B — FIFA Player Ratings:
1. Player Lookup Card
2. Top 5 Per Position
3. Value vs Rating Analysis
4. Club Squad Analyzer
5. Nationality Strength Index

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
