## Challenge link: https://platform.stratascratch.com/coding/10322-finding-user-purchases?code_type=1

# Challenge: stratascratch - Finding User Purchases

![image](https://github.com/user-attachments/assets/39cb4264-afa6-4d89-9238-bd77b675b87c)


Write a query that'll identify returning active users. A returning active user is a user that has made a second purchase within 7 days of any other of their purchases. Output a list of user_ids of these returning active users.

![image](https://github.com/user-attachments/assets/f9e97a7b-341b-4421-b4fc-6ab4231c3b97)


# Answer:

``` sql
SELECT
    DISTINCT(amazon_transactions1.user_id)
FROM 
    amazon_transactions AS amazon_transactions1
JOIN
    amazon_transactions AS amazon_transactions2
ON 
    amazon_transactions1.user_id = amazon_transactions2.user_id
    AND amazon_transactions1.id <> amazon_transactions2.id 
        AND amazon_transactions2.created_at::date-amazon_transactions1.created_at:: date
        BETWEEN 0 AND 7
GROUP BY 
    amazon_transactions1.user_id
ORDER BY 
    amazon_transactions1.user_id```
