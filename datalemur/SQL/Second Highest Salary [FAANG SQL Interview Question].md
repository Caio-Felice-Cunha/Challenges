## Challenge link: https://datalemur.com/questions/sql-second-highest-salary

# Challenge: Second Highest Salary [FAANG SQL Interview Question]

Imagine you're an HR analyst at a tech company tasked with analyzing employee salaries. Your manager is keen on understanding the pay distribution and asks you to determine the second highest salary among all employees.

It's possible that multiple employees may share the same second highest salary. In case of duplicate, display the salary only once.

![image](https://github.com/user-attachments/assets/39bd3b0f-03a8-4f6c-9c0f-b967d4f2cfb0)

![image](https://github.com/user-attachments/assets/c880db67-3453-4204-a94b-57e4d2274b31)



# Answer:

``` sql
SELECT
  salary AS Second_highest_salary
FROM(
  SELECT 
    Employee_id
    , salary
    , DENSE_RANK() OVER(
      ORDER BY salary DESC) AS Ranks
  FROM 
    employee
  ORDER BY
    salary DESC) AS Ranked
WHERE
  Ranks = 2
;
```

![image](https://github.com/user-attachments/assets/737f80e0-fc62-4ff9-8d3d-c2f39b1074a5)
