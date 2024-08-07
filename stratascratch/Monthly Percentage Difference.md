## Challenge link: https://platform.stratascratch.com/coding/10319-monthly-percentage-difference?code_type=1

# Challenge: Monthly Percentage Difference
![image](https://github.com/user-attachments/assets/51dc0a6a-eb0e-4abe-bc21-0b7990c2fe60)

Given a table of purchases by date, calculate the month-over-month percentage change in revenue. The output should include the year-month date (YYYY-MM) and percentage change, rounded to the 2nd decimal point, and sorted from the beginning of the year to the end of the year.
The percentage change column will be populated from the 2nd month forward and can be calculated as ((this month's revenue - last month's revenue) / last month's revenue)*100.

![image](https://github.com/user-attachments/assets/a7ebb69f-779e-42ce-97a7-1f6692048c96)


# Answer:

``` sql
WITH Total_Month AS ( 
    SELECT 
        TO_CHAR(created_at, 'YYYY-MM') AS Year_month,
        SUM(value) AS Total
    FROM 
        sf_transactions
    GROUP BY 
        TO_CHAR(created_at, 'YYYY-MM')
    ORDER BY
        TO_CHAR(created_at, 'YYYY-MM') DESC
)
SELECT 
    Year_month,
    ROUND(
            ((Total - LAG(Total) OVER (ORDER BY Year_month)) * 100.0) 
            / NULLIF(LAG(Total) OVER (ORDER BY Year_month), 0), 2
        ) AS Dif_Prev_Month
FROM 
    Total_Month
ORDER BY
    Year_month ASC;

```

![image](https://github.com/user-attachments/assets/3d0a59ec-32d3-4eeb-be41-adff7e967961)
