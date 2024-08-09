## Challenge link: https://datalemur.com/questions/sql-highest-grossing

# Challenge: Highest-Grossing Items [Amazon SQL Interview Question]

This is the same question as problem #12 in the SQL Chapter of Ace the Data Science Interview!

Assume you're given a table containing data on Amazon customers and their spending on products in different category, write a query to identify the top two highest-grossing products within each category in the year 2022. The output should include the category, product, and total spend.

![image](https://github.com/user-attachments/assets/9baf66a4-1e87-445a-a238-5c3a0c2019d8)

![image](https://github.com/user-attachments/assets/adb77503-d086-446b-8b07-dac4121cd9ca)

# Answer:

``` sql
SELECT
  category
  , product
  , Total
FROM(
  SELECT
    category
    , product
    , SUM(spend) Total
    , RANK()
        OVER(PARTITION BY category
        ORDER BY SUM(spend) desc) AS Ranks
  FROM 
    product_spend
  WHERE 
    EXTRACT(YEAR FROM transaction_date) = 2022
  GROUP BY
    product
    , category) AS ProductRanks
WHERE
  Ranks IN (1,2)
;
```

![image](https://github.com/user-attachments/assets/54910fb1-02e9-4261-b7a1-4ec53edd7640)
