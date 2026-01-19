## Challenge link: https://platform.stratascratch.com/coding/9917-average-salaries?code_type=4

# Challenge: Average Salaries ID 9917
Compare each employee's salary with the average salary of the corresponding department.
Output the department, first name, and salary of employees along with the average salary of that department.

Expected Output

<img width="1304" height="453" alt="image" src="https://github.com/user-attachments/assets/8edd8086-d2b4-45d3-a942-02d2f8006d7d" />


Employee Table

<img width="572" height="693" alt="image" src="https://github.com/user-attachments/assets/e7797272-4851-4677-b7b5-e92e99882624" />


Employee Table

<img width="1285" height="428" alt="image" src="https://github.com/user-attachments/assets/8a971355-246a-4b5e-b58e-d04e5d23beae" />


# Answer:

``` r
# Import your libraries
library(dplyr)

# Start writing code
head(employee)

result <- employee %>%
    group_by(department) %>%
    mutate(avg_sal = mean(salary)) %>%
    ungroup() %>%
    arrange(department) %>%
    select(department, first_name, salary, avg_sal)
``` 

### Result:

<img width="2292" height="1312" alt="image" src="https://github.com/user-attachments/assets/adf43a30-4ad3-4098-8571-7af018e1be2e" />



## Explanation:

Below I provide a **detailed, step-by-step explanation** of the solution, a **line-by-line breakdown of the code**, a **data analysis of the results**, and a **final commentary with an alternative analytical approach**.

---

## 1. Problem understanding and objective

The challenge **“Average Salaries” (ID 9917)** from StrataScratch asks me to:

> Compare each employee’s salary with the **average salary of their corresponding department**, and output:

* department
* first_name
* salary
* average salary of that department

The key analytical requirement is **not filtering**, ranking, or categorizing employees, but rather **augmenting each employee record with a departmental aggregate metric**.

This type of problem tests:

* Group-level aggregation
* Row-level enrichment
* Understanding of how group statistics can be merged back into individual observations

Source: StrataScratch – *Average Salaries*

---

## 2. High-level solution strategy

My approach follows a classic **split–apply–combine** paradigm:

1. **Split** the data by department
2. **Apply** an aggregation function (mean salary) to each department
3. **Combine** the aggregated value back into each row of the original dataset

In `dplyr`, this is achieved very efficiently using:

* `group_by()`
* `mutate()`

Unlike a join-based approach, this avoids creating an intermediate summary table.

---

## 3. Code walkthrough (line by line)

```r
library(dplyr)
```

I begin by loading **dplyr**, which provides a grammar of data manipulation optimized for readability and pipeline-based workflows. This is the standard library for tidy data operations in R.

---

```r
head(employee)
```

I inspect the dataset to verify:

* Column names
* Data types
* Presence of `department`, `salary`, and `first_name`

This step is exploratory and defensive, ensuring the schema matches expectations before transformation.

---

```r
result <- employee %>%
```

Here I initiate a **pipeline** (`%>%`), passing the `employee` data frame sequentially through multiple transformations. This avoids intermediate objects and keeps the logic linear and readable.

---

```r
    group_by(department) %>%
```

This is the most critical step conceptually.

* I partition the dataset by `department`
* All subsequent aggregation-aware functions operate **within each department**

At this point, no computation happens yet; grouping only changes how later verbs behave.

---

```r
    mutate(avg_sal = mean(salary)) %>%
```

This line performs the core computation.

* `mean(salary)` is calculated **within each department group**
* `mutate()` replicates the resulting departmental average across all rows belonging to that department

This is different from `summarise()`, which would collapse rows. Here, I intentionally preserve **row granularity**.

---

```r
    ungroup() %>%
```

I explicitly remove the grouping structure.

This is a best practice because:

* It prevents unintended grouped behavior in later steps
* It ensures sorting and selection behave globally

---

```r
    arrange(department) %>%
```

I sort the output alphabetically by department.

This step is not required for correctness, but:

* Improves readability
* Matches the expected output format shown in the challenge

---

```r
    select(department, first_name, salary, avg_sal)
```

Finally, I project only the required columns, ensuring:

* No extraneous fields appear in the final output
* The schema exactly matches the expected result

---

## 4. Why this solution is correct and efficient

### Logical correctness

* Each employee retains their individual salary
* Each employee is paired with the correct department-level average
* No rows are lost or duplicated

### Computational efficiency

* Only one pass over the data
* No joins or subqueries
* Time complexity is linear in the number of rows

This pattern is idiomatic **tidyverse R** and scales well for large datasets.

---

## 5. Data analysis of the results

### Department-level averages

From the output, I can derive the following:

| Department | Avg Salary |
| ---------- | ---------- |
| Audit      | 950        |
| Management | 175,000    |
| Sales      | 1,336.36   |

#### Audit

* Salaries range from 700 to 1,100
* The average (950) sits close to the center
* Low dispersion indicates a relatively flat pay structure

#### Management

* Salaries range from 100,000 to 250,000
* The mean is heavily influenced by high earners
* This suggests **right skewness**, where a few very high salaries pull the average upward

#### Sales

* Salaries range from 1,000 to 2,200
* Larger number of employees
* The average (1,336.36) masks substantial variation between junior and senior roles

---

## 6. Analytical interpretation

By comparing individual salaries to departmental averages, I can immediately identify:

* Employees **above average**, potentially top performers or senior roles
* Employees **below average**, potentially junior or underpaid relative to peers
* Departments with **high salary dispersion**, indicating role stratification

This output is a foundational building block for:

* Compensation benchmarking
* Pay equity analysis
* Outlier detection

---

## 7. Further or alternative analysis (extended perspective)

### Alternative 1: Join-based approach

An alternative solution would be:

1. Create a summarized table:

   ```r
   employee %>%
     group_by(department) %>%
     summarise(avg_sal = mean(salary))
   ```
2. Join it back to the original table using `left_join()`

This approach is more verbose but:

* Makes the aggregation step more explicit
* Can be preferable in SQL-heavy workflows

However, it introduces:

* An extra object
* Additional memory overhead

---

### Alternative 2: Relative salary index

A more advanced analytical extension would be to compute a **relative salary index**:

```r
salary_index = salary / avg_sal
```

This allows me to:

* Normalize salaries across departments
* Compare compensation fairness across heterogeneous teams
* Identify under- or over-compensated employees independently of department scale

For example:

* A salary index > 1 indicates above-average pay
* A salary index < 1 indicates below-average pay

This transformation enables cross-department comparisons that raw salary values cannot support.

---

## 8. Final comment

I consider this solution **clean, idiomatic, and analytically sound**. It directly addresses the problem without unnecessary complexity, while also producing a dataset that can be immediately reused for deeper compensation analysis.

The use of `group_by()` + `mutate()` is particularly powerful here, as it preserves row-level detail while embedding meaningful group-level context—a pattern that is central to real-world analytical workflows.

---

### References

* StrataScratch – *Average Salaries (ID 9917)*
* Wickham et al., *dplyr: A Grammar of Data Manipulation*, RStudio Documentation

