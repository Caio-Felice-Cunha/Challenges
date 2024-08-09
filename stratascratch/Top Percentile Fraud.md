## Challenge link: https://platform.stratascratch.com/coding/10303-top-percentile-fraud?code_type=1

# Challenge: Top Percentile Fraud

ABC Corp is a mid-sized insurer in the US and in the recent past their fraudulent claims have increased significantly for their personal auto insurance portfolio. They have developed a ML based predictive model to identify propensity of fraudulent claims. Now, they assign highly experienced claim adjusters for top 5 percentile of claims identified by the model.
Your objective is to identify the top 5 percentile of claims from each state. Your output should be policy number, state, claim cost, and fraud score.

![image](https://github.com/user-attachments/assets/ab5d9cf6-9426-485d-8a3d-5128cb1878af)


# Answer:

``` sql
WITH percentiles AS
  (SELECT 
    state
    , percentile_cont(0.05) 
        within GROUP (
            ORDER BY fraud_score DESC
            ) AS percentile
   FROM 
        fraud_score
   GROUP BY 
        state)
SELECT 
    policy_num
    , fraud_score.state
    , claim_cost
    , fraud_score
FROM 
    fraud_score
JOIN 
    percentiles 
ON 
    fraud_score.state = percentiles.state
WHERE 
    fraud_score >= percentile
```

![image](https://github.com/user-attachments/assets/c6897836-25c3-4c85-b30d-d1dc53e599f5)
