## Challenge link: https://datalemur.com/questions/non-profitable-drugs

# Challenge: datalemur

CVS Health is analyzing its pharmacy sales data, and how well different products are selling in the market. Each drug is exclusively manufactured by a single manufacturer.

Write a query to identify the manufacturers associated with the drugs that resulted in losses for CVS Health and calculate the total amount of losses incurred.

Output the manufacturer's name, the number of drugs associated with losses, and the total losses in absolute value. Display the results sorted in descending order with the highest losses displayed at the top.

If you like this question, try out Pharmacy Analytics (Part 3)!

![image](https://github.com/user-attachments/assets/63e154a6-9250-45d4-94bb-62b8eb45c504)


# Answer:

``` sql
SELECT
  manufacturer,
  COUNT(drug) AS drug_count,
  ABS(SUM(total_sales- cogs)) AS Total
FROM
  pharmacy_sales
WHERE
  total_sales- cogs <= 0
GROUP BY
  manufacturer
ORDER BY
  Total DESC;
```
