## Challenge link: https://datalemur.com/questions/sql-top-three-salaries

# Challenge: Top Three Salaries [FAANG SQL Interview Question]

As part of an ongoing analysis of salary distribution within the company, your manager has requested a report identifying high earners in each department. A 'high earner' within a department is defined as an employee with a salary ranking among the top three salaries within that department.

You're tasked with identifying these high earners across all departments. Write a query to display the employee's name along with their department name and salary. In case of duplicates, sort the results of department name in ascending order, then by salary in descending order. If multiple employees have the same salary, then order them alphabetically.

Note: Ensure to utilize the appropriate ranking window function to handle duplicate salaries effectively.

As of June 18th, we have removed the requirement for unique salaries and revised the sorting order for the results.

![image](https://github.com/user-attachments/assets/d4d30a17-3376-441f-882a-dc6cc3e7edb7)  ![image](https://github.com/user-attachments/assets/ed3af448-a181-46e7-ab2d-454bef26fce3)

![image](https://github.com/user-attachments/assets/67192dd2-b8fa-4839-988a-a496a70f5b3c)


# Answer:

``` sql
SELECT
  department_name
  , name
  , salary
FROM(
  SELECT
   department.department_name
   , employee.name
   , employee.salary
   , DENSE_RANK()
      OVER(
        PARTITION BY department.department_name
        ORDER BY employee.salary DESC) AS Salary_Depart_Rank
  FROM
   employee
  JOIN 
    department
  ON 
    employee.department_id = department.department_id) AS MyTable

WHERE
  Salary_Depart_Rank <= 3 
ORDER BY 
  department_name ASC
  , salary DESC 
  , name ASC;
``` 
![image](https://github.com/user-attachments/assets/736305ba-0371-422d-ae01-8fe0d7328aba)



## Explanation:
### Challenge Breakdown

The challenge is to identify the top three highest-paid employees in each department of a company. We need to list the employee's name, their department, and their salary. If there are employees with the same salary, the ranking should handle ties appropriately. Finally, the results should be ordered by department name, salary (in descending order), and employee name (alphabetically).

### Code Explanation

- The outer query filters out the top three earners (`Salary_Depart_Rank <= 3`) from each department and orders the results as required:
  - `department_name` in ascending order.
  - `salary` in descending order.
  - `name` in ascending order (alphabetically).

#### 2. **Subquery - Using DENSE_RANK()**

```sql
SELECT
  department.department_name,
  employee.name,
  employee.salary,
  DENSE_RANK() OVER(
    PARTITION BY department.department_name
    ORDER BY employee.salary DESC
  ) AS Salary_Depart_Rank
FROM
  employee
JOIN 
  department
ON 
  employee.department_id = department.department_id
```

- **Joins**: The subquery joins the `employee` table with the `department` table using `department_id` to get each employee's department name.
- **DENSE_RANK()**: This window function ranks employees within each department based on their salary:
  - `PARTITION BY department.department_name`: This clause groups the ranking by department.
  - `ORDER BY employee.salary DESC`: This ranks the salaries in descending order, meaning the highest salary gets rank 1.
  - `DENSE_RANK()` ensures that employees with the same salary receive the same rank, and the next rank is sequential (i.e., no gaps in ranking numbers).

#### 3. **Filtering Top 3 Salaries**

- The `WHERE` clause in the outer query filters the results to only include employees whose rank (`Salary_Depart_Rank`) is 3 or less, representing the top three earners in each department.

### Summary

This SQL query effectively identifies the top three salaries within each department using the `DENSE_RANK()` window function to handle ties. It then filters and orders the results according to the specified criteria.
