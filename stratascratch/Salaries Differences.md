## Challenge link: https://platform.stratascratch.com/coding/10308-salaries-differences?code_type=1

# Challenge: Salaries Differences
Write a query that calculates the difference between the highest salaries found in the marketing and engineering departments. Output just the absolute difference in salaries.

![image](https://github.com/user-attachments/assets/a88af4f8-b5e1-4fb5-bf49-cbc41fce62e6)

![image](https://github.com/user-attachments/assets/fd7726f4-b8e9-401f-b972-c4fc2624912d)



# Answer:

``` sql
SELECT 
    ABS(
     MAX(db_employee.salary) 
      FILTER (
       WHERE db_dept.department = 'marketing')
    -
     MAX(db_employee.salary)
      FILTER
       (WHERE db_dept.department = 'engineering')
   ) AS salary_difference
FROM 
    db_employee
LEFT JOIN 
    db_dept
ON
    db_employee.department_id = db_dept.id;
```

![image](https://github.com/user-attachments/assets/e0470190-8af5-476a-a7e2-156fa88ccf07)
