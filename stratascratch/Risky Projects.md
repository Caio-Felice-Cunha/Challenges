## Challenge link: https://platform.stratascratch.com/coding/10304-risky-projects?code_type=1

# Challenge: Risky Projects

![image](https://github.com/user-attachments/assets/d856fbe8-9580-4805-930d-9fda61c2cc53)


Identify projects that are at risk for going overbudget. A project is considered to be overbudget if the cost of all employees assigned to the project is greater than the budget of the project.


You'll need to prorate the cost of the employees to the duration of the project. For example, if the budget for a project that takes half a year to complete is $10K, then the total half-year salary of all employees assigned to the project should not exceed $10K. Salary is defined on a yearly basis, so be careful how to calculate salaries for the projects that last less or more than one year.


Output a list of projects that are overbudget with their project name, project budget, and prorated total employee expense (rounded to the next dollar amount).


HINT: to make it simpler, consider that all years have 365 days. You don't need to think about the leap years.

![image](https://github.com/user-attachments/assets/98caa40c-ec4b-45e6-8942-69aedfd02ce5)

![image](https://github.com/user-attachments/assets/cdecf78a-9026-430c-b32d-0159e03f4df0)

![image](https://github.com/user-attachments/assets/3b76f9a1-589b-414c-bdf4-35764ea9dcb6)


# Answer:

``` sql
SELECT 
    title
    , budget
    , CEILING(prorated_expenses) AS prorated_employee_expense
FROM
    (SELECT 
        title
        , budget
        , (end_date::date - start_date::date) * (SUM(salary) / 365) AS prorated_expenses
     FROM 
        linkedin_projects
     INNER JOIN 
        linkedin_emp_projects
     ON 
        linkedin_projects.id = linkedin_emp_projects.project_id
     INNER JOIN 
        linkedin_employees
     ON 
        linkedin_emp_projects.emp_id = linkedin_employees.id
     GROUP BY 
        title
        , budget
        , end_date
        , start_date
    ) AS subquery
WHERE 
    prorated_expenses > budget
ORDER BY 
    title ASC;
```

![image](https://github.com/user-attachments/assets/db5bc4cf-2d97-47fc-955d-8b62cebf9bf5)
