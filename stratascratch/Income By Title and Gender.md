## Challenge link: https://platform.stratascratch.com/coding/10077-income-by-title-and-gender?code_type=3

# Challenge: Income By Title and Gender

![image](https://github.com/user-attachments/assets/d9d9d3a9-1cb5-4345-9789-9f0fe02514a4)

Find the average total compensation based on employee titles and gender. Total compensation is calculated by adding both the salary and bonus of each employee. However, not every employee receives a bonus so disregard employees without bonuses in your calculation. Employee can receive more than one bonus.
Output the employee title, gender (i.e., sex), along with the average total compensation.

![image](https://github.com/user-attachments/assets/bfd8cfdd-592f-4510-a311-1a7fb356a383)


# Answer:

``` sql
SELECT 
    sf_employee.employee_title
    , sf_employee.sex
    , AVG(sf_employee.salary + Bonus_Empl_Tb.Bonus_Worker) AS Avg_Comp
FROM 
    sf_employee
INNER JOIN 
        (SELECT
            worker_ref_id
            , SUM(bonus) AS Bonus_Worker
        FROM 
            sf_bonus
        GROUP BY
            worker_ref_id) Bonus_Empl_Tb
            
        ON sf_employee.id = Bonus_Empl_Tb.worker_ref_id
GROUP BY 
    sf_employee.employee_title
    , sf_employee.sex
``` 

### Result:

![image](https://github.com/user-attachments/assets/41857276-e73f-40bc-886f-bcbed424fdba)


## Explanation:
Let's break down the SQL code used to solve the challenge of finding the average total compensation by employee title and gender. The challenge involves calculating the total compensation by summing the salary and bonuses of employees. Employees who do not receive a bonus are excluded from the calculation. The final result should display the employee title, gender, and the average total compensation.

### Breakdown of the Code

#### 1. **SELECT Clause**

```sql
SELECT 
    sf_employee.employee_title,
    sf_employee.sex,
    AVG(sf_employee.salary + Bonus_Empl_Tb.Bonus_Worker) AS Avg_Comp
```
- **`sf_employee.employee_title`**: This column selects the title of each employee from the `sf_employee` table.
- **`sf_employee.sex`**: This column selects the gender (sex) of each employee from the `sf_employee` table.
- **`AVG(sf_employee.salary + Bonus_Empl_Tb.Bonus_Worker)`**: This expression calculates the average of the total compensation for employees, which is the sum of the salary and the bonus. The `AVG` function is used to compute the average value of this sum.
- **`AS Avg_Comp`**: This aliases the calculated average total compensation as `Avg_Comp` for better readability in the output.

#### 2. **FROM Clause**

```sql
FROM 
    sf_employee
```
- This specifies the `sf_employee` table as the primary source of data, which contains information about employees including their titles, salaries, and genders.

#### 3. **INNER JOIN Clause**

```sql
INNER JOIN 
    (SELECT
        worker_ref_id,
        SUM(bonus) AS Bonus_Worker
    FROM 
        sf_bonus
    GROUP BY
        worker_ref_id) Bonus_Empl_Tb
    ON sf_employee.id = Bonus_Empl_Tb.worker_ref_id
```
- **Subquery**:
  - **`SELECT worker_ref_id, SUM(bonus) AS Bonus_Worker`**: The subquery selects the `worker_ref_id` and sums up the bonuses for each employee, aliasing the sum as `Bonus_Worker`.
  - **`FROM sf_bonus`**: Specifies the `sf_bonus` table, which contains information about employee bonuses.
  - **`GROUP BY worker_ref_id`**: Groups the bonuses by the `worker_ref_id`, ensuring that bonuses for the same employee are aggregated.
  
- **`Bonus_Empl_Tb`**: The result of the subquery is aliased as `Bonus_Empl_Tb`.
- **`INNER JOIN ... ON sf_employee.id = Bonus_Empl_Tb.worker_ref_id`**: The `INNER JOIN` links the `sf_employee` table with the result of the subquery (`Bonus_Empl_Tb`) using the `worker_ref_id` field. This ensures only employees with a bonus are included in the calculation.

#### 4. **GROUP BY Clause**

```sql
GROUP BY 
    sf_employee.employee_title,
    sf_employee.sex
```
- **`GROUP BY`**: This groups the results by both `employee_title` and `sex`. The `AVG` function then calculates the average total compensation for each unique combination of title and gender.

### Data Analysis of the Result

Let's analyze the output:

```plaintext
employee_title  sex Total_Comp
Senior Sales    M   5350
Auditor         M   2200
Sales           M   4600
Manager         F   209500
```

#### Observations:
1. **Senior Sales (Male)**: The average total compensation for male employees with the title "Senior Sales" is 5,350.
2. **Auditor (Male)**: The average total compensation for male employees with the title "Auditor" is 2,200.
3. **Sales (Male)**: The average total compensation for male employees with the title "Sales" is 4,600.
4. **Manager (Female)**: The average total compensation for female employees with the title "Manager" is 209,500.

#### Comment:
- The output suggests that there is a significant disparity in total compensation, particularly for the "Manager" role, where female employees seem to receive an exceptionally higher average total compensation compared to other roles. This could indicate a few highly compensated individuals or possibly a data entry error. It's important to delve deeper into this data to understand the underlying reasons.

### Further or Alternative Analysis

**Alternative Analysis 1: Median Compensation**
- Instead of calculating the average (`AVG`), you might calculate the median total compensation for a more robust measure that is less sensitive to outliers. This would help identify if the high average compensation for female managers is skewed by a small number of highly paid individuals.

**Alternative Analysis 2: Compensation Distribution**
- Investigate the distribution of total compensation across different roles and genders. This could involve plotting histograms or box plots to visualize how compensation is distributed and whether certain roles or genders have more variability.

**Alternative Analysis 3: Bonus Impact**
- Analyze the impact of bonuses separately by calculating the average bonus for each title and gender. This could reveal whether the bonuses contribute significantly to the disparity in total compensation.

### Example of Alternative Query (Median Compensation):

```sql
WITH Compensation_CTE AS (
    SELECT 
        sf_employee.employee_title,
        sf_employee.sex,
        (sf_employee.salary + Bonus_Empl_Tb.Bonus_Worker) AS Total_Comp
    FROM 
        sf_employee
    INNER JOIN 
        (SELECT
            worker_ref_id,
            SUM(bonus) AS Bonus_Worker
        FROM 
            sf_bonus
        GROUP BY
            worker_ref_id) Bonus_Empl_Tb
    ON sf_employee.id = Bonus_Empl_Tb.worker_ref_id
)
SELECT
    employee_title,
    sex,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY Total_Comp) AS Median_Comp
FROM
    Compensation_CTE
GROUP BY
    employee_title,
    sex
```

### Explanation:
- **`WITH Compensation_CTE AS ...`**: Creates a common table expression (CTE) that calculates the total compensation for each employee.
- **`PERCENTILE_CONT(0.5)`**: This function calculates the median (50th percentile) total compensation for each group.
- **`WITHIN GROUP (ORDER BY Total_Comp)`**: Specifies that the median should be calculated based on the `Total_Comp` values.

This alternative approach could give you a more accurate picture of central tendencies within the data, especially when dealing with potentially skewed distributions.
