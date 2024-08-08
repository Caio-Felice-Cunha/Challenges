## Challenge link: https://datalemur.com/questions/frequent-callers

# Challenge: Patient Support Analysis (Part 1) [UnitedHealth SQL Interview Question]

UnitedHealth Group (UHG) has a program called Advocate4Me, which allows policy holders (or, members) to call an advocate and receive support for their health care needs – whether that's claims and benefits support, drug coverage, pre- and post-authorisation, medical records, emergency assistance, or member portal services.

Write a query to find how many UHG policy holders made three, or more calls, assuming each call is identified by the case_id column.

![image](https://github.com/user-attachments/assets/cfb6d7ca-0e28-4567-aa35-46f7293e48e1)

![image](https://github.com/user-attachments/assets/00a06163-1e5d-4085-b8a7-f424073dbb3c)


# Answer:

``` sql
SELECT
  COUNT(*)
FROM (
  SELECT 
    policy_holder_id,
    COUNT(case_id )
    
  FROM 
    callers
  
  GROUP BY
    policy_holder_id
    
  HAVING
    COUNT(case_id) >= 3) AS More_3_Calls;
```

![image](https://github.com/user-attachments/assets/4ac97875-4901-44c8-b2be-92d16526093c)
