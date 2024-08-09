## Challenge link: https://datalemur.com/questions/time-spent-snaps

# Challenge: Sending vs. Opening Snaps [Snapchat SQL Interview Question]

This is the same question as problem #25 in the SQL Chapter of Ace the Data Science Interview!

Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

Notes:

Calculate the following percentages:
time spent sending / (Time spent sending + Time spent opening)
Time spent opening / (Time spent sending + Time spent opening)
To avoid integer division in percentages, multiply by 100.0 and not 100.
Effective April 15th, 2023, the solution has been updated and optimised.

![image](https://github.com/user-attachments/assets/c7d8a6d5-568a-4748-bcbe-2ff58274dcba)

![image](https://github.com/user-attachments/assets/9bd122cb-c849-4dbb-a3f0-bde6ea7e5aec)

# Answer:

``` sql
SELECT 
  age.age_bucket 
  , ROUND(
      SUM(activities.time_spent) 
        FILTER (
          WHERE activities.activity_type = 'send')
      / SUM(activities.time_spent) * 100,2) 
    AS Percentage_Send 
          
  , ROUND(
      SUM(activities.time_spent) 
        FILTER (
          WHERE activities.activity_type = 'open')
      / SUM(activities.time_spent) * 100,2) 
    AS Percentage_Open
          
FROM 
  activities

INNER JOIN 
  age_breakdown AS age 
ON 
  activities.user_id = age.user_id 
  
WHERE 
  activities.activity_type IN ('send', 'open') 
GROUP BY 
  age.age_bucket;
```
![image](https://github.com/user-attachments/assets/bda8ab38-b884-4c42-957c-f1b55ffaea45)
