## Challenge link: https://datalemur.com/questions/total-drugs-sales

# Challenge: Pharmacy Analytics (Part 3) [CVS Health SQL Interview Question]

CVS Health wants to gain a clearer understanding of its pharmacy sales and the performance of various products.

Write a query to calculate the total drug sales for each manufacturer. Round the answer to the nearest million and report your results in descending order of total sales. In case of any duplicates, sort them alphabetically by the manufacturer name.

Since this data will be displayed on a dashboard viewed by business stakeholders, please format your results as follows: "$36 million".

![image](https://github.com/user-attachments/assets/dae7ba61-875e-4754-858e-e122c183d311)


# Answer:

``` sql
SELECT 
  manufacturer,
  CONCAT(
    '$',
    ROUND((SUM(total_sales) / 1000000)),
    ' million') AS Total_sales
FROM 
  pharmacy_sales
GROUP BY
  manufacturer
ORDER BY
  SUM(total_sales) DESC,
  manufacturer ASC;
```

![image](https://github.com/user-attachments/assets/0953fd45-8dd2-4cdf-8c1f-482ca69a8f77)
