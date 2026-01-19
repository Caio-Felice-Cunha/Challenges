## Challenge link: https://datalemur.com/questions/matching-skills

# Challenge: Data Science Skills - LinkedIn SQL Interview Question

Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.

Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.

Assumption:

There are no duplicates in the candidates table.


candidates Table:


candidates Example Input:


# Answer:

``` sql
SELECT 
    candidate_id AS PerfectCandidate
FROM 
  candidates
WHERE 
  skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY 
  candidate_id
HAVING 
  COUNT(skill) = 3
ORDER BY 
  candidate_id ASC;
``` 

### Result:

## Explanation:

Below is the same analysis rewritten **entirely in the first person**, while preserving a **technical, factual, and hyper-informative style**.

---

## 1. Problem Restatement (How I Interpret the Challenge)

I am given a relational table called `candidates` where:

* Each row represents **one skill owned by one candidate**
* A single candidate can appear in multiple rows
* There are **no duplicate (candidate_id, skill) pairs**

My objective is to identify candidates who fully meet the requirements for a Data Science role. Specifically, I must return only those candidates who possess **all three required skills**:

* Python
* Tableau
* PostgreSQL

This problem is best framed as a **set containment problem** in SQL:
I need to find candidates whose set of skills completely contains the required subset.

---

## 2. SQL Solution I Use

```sql
SELECT 
    candidate_id AS PerfectCandidate
FROM 
    candidates
WHERE 
    skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY 
    candidate_id
HAVING 
    COUNT(skill) = 3
ORDER BY 
    candidate_id ASC;
```

---

## 3. Detailed Breakdown of the Query

### 3.1 Selecting the Candidate Identifier

```sql
SELECT candidate_id AS PerfectCandidate
```

I select `candidate_id` because I want one output row per candidate.
I rename the column to `PerfectCandidate` to make the semantic meaning explicit: these are candidates who fully match the job requirements.

---

### 3.2 Specifying the Source Table

```sql
FROM candidates
```

I pull the data from the `candidates` table, which is structured in a **long format**, meaning each skill is stored in a separate row.

---

### 3.3 Filtering Only Relevant Skills

```sql
WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
```

At this step, I eliminate all irrelevant skills.
This ensures I only evaluate skills that matter for the job.

After this filter:

* Candidates with unrelated skills are ignored
* Candidates with partial overlap remain temporarily

This step **reduces noise** and simplifies the aggregation logic.

---

### 3.4 Grouping by Candidate

```sql
GROUP BY candidate_id
```

I group the data by `candidate_id` so I can analyze each candidate as a unit.
This allows me to count how many required skills each candidate possesses.

---

### 3.5 Enforcing Skill Completeness

```sql
HAVING COUNT(skill) = 3
```

This is the most important condition in the query.

Because:

* I already filtered to only the required skills
* There are exactly **three required skills**
* The problem states there are **no duplicates**

A count of `3` guarantees that the candidate has **all required skills**.

I use `HAVING` instead of `WHERE` because aggregate functions operate after grouping.

---

### 3.6 Ordering the Final Output

```sql
ORDER BY candidate_id ASC;
```

I sort the result in ascending order to produce a clean, deterministic output.

---

## 4. Data Analysis of the Result

### Original Input Data

| candidate_id | skill      |
| ------------ | ---------- |
| 123          | Python     |
| 123          | Tableau    |
| 123          | PostgreSQL |
| 234          | R          |
| 234          | PowerBI    |
| 234          | SQL Server |
| 345          | Python     |
| 345          | Tableau    |

---

### After Applying the `WHERE` Filter

| candidate_id | skill      |
| ------------ | ---------- |
| 123          | Python     |
| 123          | Tableau    |
| 123          | PostgreSQL |
| 345          | Python     |
| 345          | Tableau    |

---

### After Grouping and Counting

| candidate_id | COUNT(skill) |
| ------------ | ------------ |
| 123          | 3            |
| 345          | 2            |

---

### Final Output

| PerfectCandidate |
| ---------------- |
| 123              |

---

## 5. My Comment on the Result

From the analysis, I conclude that:

* Candidate **123** meets **100% of the job requirements**
* Candidate **345** is partially qualified but missing PostgreSQL
* Candidate **234** has no overlap with the required skill set and is excluded early

This solution is:

* Efficient in execution
* Easy to reason about
* Highly suitable for SQL interviews
* Correct under the assumption of no duplicate skills per candidate

---

## 6. Further / Alternative Analysis

### 6.1 Alternative Solution Using Conditional Aggregation

If I want a more defensive or explicit approach, I can write:

```sql
SELECT 
    candidate_id
FROM 
    candidates
GROUP BY 
    candidate_id
HAVING 
    SUM(CASE WHEN skill = 'Python' THEN 1 ELSE 0 END) > 0
AND SUM(CASE WHEN skill = 'Tableau' THEN 1 ELSE 0 END) > 0
AND SUM(CASE WHEN skill = 'PostgreSQL' THEN 1 ELSE 0 END) > 0
ORDER BY 
    candidate_id;
```

---

### 6.2 Why This Alternative Works

In this version, I explicitly verify the presence of **each required skill independently**.
This makes the logic more verbose but also more robust.

Even if:

* Duplicate skills exist
* Additional skills are added
* Data quality assumptions break

The query still produces correct results.

---

### 6.3 Comparison of the Two Approaches

| Aspect                | COUNT + IN       | Conditional Aggregation |
| --------------------- | ---------------- | ----------------------- |
| Conciseness           | High             | Moderate                |
| Explicitness          | Medium           | High                    |
| Duplicate safety      | Assumption-based | Fully safe              |
| Production robustness | Medium           | High                    |

---

## 7. Final Insight

I recognize this problem as a classic example of **relational division** in SQL.
My original solution is optimal for interviews and clean datasets, while the alternative approach is often preferable in production environments where data assumptions may not always hold.