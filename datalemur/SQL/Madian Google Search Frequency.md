## Challenge link: https://datalemur.com/questions/median-search-freq

# Challenge: Median Google Search Frequency

Google's marketing team is making a Superbowl commercial and needs a simple statistic to put on their TV ad: the median number of searches a person made last year.

However, at Google scale, querying the 2 trillion searches is too costly. Luckily, you have access to the summary table which tells you the number of searches made last year and how many Google users fall into that bucket.

Write a query to report the median of searches made by a user. Round the median to one decimal point.


<img width="381" height="579" alt="Screenshot 2026-01-22 075115" src="https://github.com/user-attachments/assets/f20ba24e-9a88-430f-9237-ece3e576540f" />


# Answer:

``` sql
WITH ordered AS (
  SELECT
    searches,
    num_users,
    SUM(num_users) OVER (ORDER BY searches) AS cum_up,
    SUM(num_users) OVER () AS total_users
  FROM search_frequency
),

ranges AS (
  SELECT
    searches,
    cum_up - num_users + 1 AS cum_lo,
    cum_up AS cum_hi,
    total_users
  FROM ordered
)

SELECT
  ROUND(AVG(val), 1) AS median
FROM (
  SELECT
    searches AS val
  FROM ranges
  WHERE
    -- if total is odd, both positions collapse to the same
    (cum_lo <= (total_users + 1) / 2 AND cum_hi >= (total_users + 1) / 2)
    OR
    -- covers even total_users by capturing both middle slots
    (cum_lo <= total_users / 2 AND cum_hi >= total_users / 2)
    OR
    (cum_lo <= total_users / 2 + 1 AND cum_hi >= total_users / 2 + 1)
) AS median_positions;


``` 

### Result:
<img width="848" height="445" alt="image" src="https://github.com/user-attachments/assets/6216e1f9-4047-4322-9e0b-b389000f5fab" />

## Explanation:

### 1. Problem Restatement (Contextual Understanding)

I am asked to compute the **median number of searches per user**, but I **do not** have access to individual-level data. Instead, I have an **aggregated frequency table**:

* `searches`: number of searches made by a user in a year
* `num_users`: how many users made exactly that many searches

At Google scale, expanding this into trillions of rows is infeasible, so the solution must compute the median **directly from the distribution**.

The statistical definition of the median drives everything:

* If the total number of users is **odd**, the median is the value at position
  ((N + 1) / 2)
* If the total number of users is **even**, the median is the **average of the values** at positions
  (N / 2) and (N / 2 + 1)

---

### 2. High-Level Strategy

My approach is to:

1. **Order the buckets** by `searches`
2. **Build cumulative user ranges** (like expanding the data conceptually, but without actually doing it)
3. **Identify which bucket(s)** contain the median position(s)
4. **Average those values** and round to one decimal place

This is a classic **weighted median** problem solved using **window functions**.

---

### 3. Step-by-Step Code Breakdown

#### 3.1 First CTE — `ordered`

```sql
WITH ordered AS (
  SELECT
    searches,
    num_users,
    SUM(num_users) OVER (ORDER BY searches) AS cum_up,
    SUM(num_users) OVER () AS total_users
  FROM search_frequency
),
```

#### What I am doing here

* `SUM(num_users) OVER (ORDER BY searches)`
  → This creates a **running cumulative count** of users as searches increase.
* `SUM(num_users) OVER ()`
  → This computes the **total number of users**, repeated on every row.

#### Why this matters

At this stage, I now know:

* How many users exist **up to and including** each `searches` bucket
* The **total population size**, which determines where the median lies

Conceptually, this mimics the sorted expanded array *without actually building it*.

---

### 3.2 Second CTE — `ranges`

```sql
ranges AS (
  SELECT
    searches,
    cum_up - num_users + 1 AS cum_lo,
    cum_up AS cum_hi,
    total_users
  FROM ordered
)
```

#### What I am doing here

For each bucket, I compute the **range of user positions** it covers:

* `cum_lo`: the **first position** in this bucket
* `cum_hi`: the **last position** in this bucket

For example, if:

* `searches = 3`
* `num_users = 3`
* cumulative users before = 4

Then this bucket covers positions **5 through 7**.

#### Why this matters

Now every row answers the question:

> “If I conceptually expanded the table, which positions would this value occupy?”

This transforms the problem into a **range containment** problem.

---

### 3.3 Final Selection — Finding Median Positions

```sql
SELECT
  ROUND(AVG(val), 1) AS median
FROM (
  SELECT
    searches AS val
  FROM ranges
  WHERE
    (cum_lo <= (total_users + 1) / 2 AND cum_hi >= (total_users + 1) / 2)
    OR
    (cum_lo <= total_users / 2 AND cum_hi >= total_users / 2)
    OR
    (cum_lo <= total_users / 2 + 1 AND cum_hi >= total_users / 2 + 1)
) AS median_positions;
```

#### Logic breakdown

I explicitly cover **both odd and even cases**:

1. **Odd total users**

   ```sql
   (total_users + 1) / 2
   ```

   This identifies the single median position.

2. **Even total users**

   ```sql
   total_users / 2
   total_users / 2 + 1
   ```

   These identify the two middle positions.

Any bucket whose range contains **either median position** is selected.

Finally:

* I take the `AVG(val)` of those bucket values
* I round to **one decimal place**, as required

---

## 4. Data Analysis of the Result

### Example Output

```text
median
3.5
```

### Interpretation

This result tells me:

* Half of users performed **≤ 3.5 searches**
* Half of users performed **≥ 3.5 searches**

Because the median is **not an integer**, I immediately know:

* The total number of users is **even**
* The two middle positions fall into **different search buckets**

This suggests a **right-skewed or uneven distribution**, where:

* Lower search counts may have high user density
* Higher search counts are thinner but influential around the midpoint

---

## 5. Statistical Commentary

From a product or marketing perspective:

* The **median** is a better statistic than the mean here
* Extremely heavy users (power users, bots, or automation) do not distort it
* This gives Google a **stable, defensible metric** for a mass-market ad

At Google scale, this is exactly the type of statistic that:

* Is computationally efficient
* Is resistant to outliers
* Communicates typical user behavior clearly

---

## 6. Further / Alternative Analysis

### Alternative 1: Percentile-Based Window Function

Some SQL engines support `PERCENTILE_CONT`, which could compute a weighted median **if expanded**, but:

* It would require data explosion or approximation
* It is **less scalable** and often disallowed in production warehouses

Thus, the current approach is **more robust and portable**.

---

### Alternative 2: Using Row-Number Simulation

Another approach is to simulate expansion using:

```sql
ROW_NUMBER() OVER (ORDER BY searches)
```

But this again assumes **row-level data**, which violates the problem constraints.

---

### Deeper Insight: Why This Solution Scales

What makes this solution elegant is that:

* Time complexity is **O(n)** over buckets, not users
* Memory usage is minimal
* It relies purely on **window aggregates**, which are heavily optimized in modern SQL engines

In real Google-scale systems (BigQuery, Presto, Spark SQL), this pattern is exactly how weighted statistics are computed safely.

---

## 7. Final Takeaway

I solved the problem by:

* Treating aggregated data as **implicit ranges**
* Locating median positions mathematically
* Avoiding any form of data expansion

The result is:

* Correct
* Efficient
* Statistically sound
* Production-ready

This is a textbook example of turning a **theoretical statistical definition** into a **scalable SQL implementation**.

