## Challenge link: https://platform.stratascratch.com/coding/10318-new-products?code_type=1

# Challenge: New Products

![image](https://github.com/user-attachments/assets/7b891403-5171-4251-bfb8-45afa736d3e1)

You are given a table of product launches by company by year. Write a query to count the net difference between the number of products companies launched in 2020 with the number of products companies launched in the previous year. Output the name of the companies and a net difference of net products released for 2020 compared to the previous year.

![image](https://github.com/user-attachments/assets/bd6e323a-be23-4598-883c-1e4cdb4deb9f)


# Answer:

``` sql
SELECT 
     company_name,
     net_products
FROM (
    SELECT 
         company_name,
         year,

         COUNT(
          DISTINCT product_name)
                    - 
          LAG(
            COUNT(
              DISTINCT product_name))
                OVER(
                  PARTITION BY company_name ORDER BY year
            ) AS net_products

    FROM 
        car_launches
    GROUP BY 
        company_name, year
) AS TB_net_products
WHERE
    net_products IS NOT NULL;
```



