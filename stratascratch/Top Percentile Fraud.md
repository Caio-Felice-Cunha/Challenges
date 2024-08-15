## Challenge link: https://platform.stratascratch.com/coding/10303-top-percentile-fraud?code_type=3

# Challenge: Top Percentile Fraud

![image](https://github.com/user-attachments/assets/6b7a6414-61a8-4d5d-8595-ebd74978bd90)

ABC Corp is a mid-sized insurer in the US and in the recent past their fraudulent claims have increased significantly for their personal auto insurance portfolio. They have developed a ML based predictive model to identify propensity of fraudulent claims. Now, they assign highly experienced claim adjusters for top 5 percentile of claims identified by the model.
Your objective is to identify the top 5 percentile of claims from each state. Your output should be policy number, state, claim cost, and fraud score.

![image](https://github.com/user-attachments/assets/1993ef0a-7312-49e2-b81e-d2fdd8c39b3b)


# Answer:

``` sql
SELECT 
    policy_num
    , state
    , claim_cost
    , fraud_score
FROM
    (SELECT 
        policy_num
        , state
        , fraud_score
        , claim_cost
        , PERCENT_RANK()
            OVER(
                PARTITION BY state
                ORDER BY fraud_score DESC) AS Percentile
    FROM 
        fraud_score) AS Rank_Table
WHERE 
    Percentile <= .05 
```
![image](https://github.com/user-attachments/assets/153ff2b0-524a-46eb-982b-86102c22e677)


## Explanation:
Let's break down the solution for identifying the top 5 percentile of claims from each state:

### Detailed Explanation of the Solution

1. **Subquery with Percentile Calculation:**

    ```sql
    SELECT 
        policy_num
        , state
        , fraud_score
        , claim_cost
        , PERCENT_RANK()
            OVER(
                PARTITION BY state
                ORDER BY fraud_score DESC) AS Percentile
    FROM 
        fraud_score
    ```

    **Explanation:**
    - This subquery selects the `policy_num`, `state`, `fraud_score`, and `claim_cost` from the `fraud_score` table.
    - The `PERCENT_RANK()` function is used to calculate the percentile rank of each claim within each state.
        - `PARTITION BY state`: This clause partitions the data by state. The percentile calculation is done within each partition (i.e., within each state).
        - `ORDER BY fraud_score DESC`: This orders the data by `fraud_score` in descending order. Claims with higher fraud scores will have higher percentile ranks.
    - `PERCENT_RANK()` computes the relative rank of a row within the partition. It returns a value between 0 and 1, representing the percentile rank of the row.

2. **Filtering Top 5 Percentile Claims:**

    ```sql
    SELECT 
        policy_num
        , state
        , claim_cost
        , fraud_score
    FROM
        (/* Subquery here */) AS Rank_Table
    WHERE 
        Percentile <= .05 
    ```

    **Explanation:**
    - The outer query filters the results of the subquery to include only those rows where the `Percentile` is less than or equal to 0.05. This selects the top 5% of claims within each state based on their fraud scores.

### Alternative Solution Using `NTILE()`

An alternative approach to identify the top 5 percentile of claims could use the `NTILE()` function instead of `PERCENT_RANK()`. The `NTILE()` function divides the data into a specified number of buckets, which can then be used to select the top 5 percentile.

**Alternative SQL Query:**

```sql
SELECT 
    policy_num
    , state
    , claim_cost
    , fraud_score
FROM
    (SELECT 
        policy_num
        , state
        , fraud_score
        , claim_cost
        , NTILE(100)
            OVER(
                PARTITION BY state
                ORDER BY fraud_score DESC) AS Percentile
    FROM 
        fraud_score) AS Rank_Table
WHERE 
    Percentile <= 5
```

**Explanation:**
- `NTILE(100)`: This function divides the ordered data into 100 buckets. Each bucket represents 1% of the data. By ordering the data in descending order of `fraud_score`, higher fraud scores will end up in lower-numbered buckets.
- `Percentile <= 5`: This filter selects the top 5 buckets, corresponding to the top 5 percentile of the data.

### Data Analysis and Conclusion

The output from the original solution and the alternative solution will both provide the top 5 percentile of claims by state. 

#### Analysis:
- Both methods will effectively identify the top 5% of claims with the highest fraud scores within each state.
- `PERCENT_RANK()` provides a continuous rank value that is more precise when dealing with fractional percentiles, while `NTILE()` provides a discrete bucket assignment, which might be more straightforward but could be less precise in boundary cases.

#### Conclusion:
- The choice between `PERCENT_RANK()` and `NTILE()` might depend on the specific requirements and data distribution. If precise percentile ranking is needed, `PERCENT_RANK()` is preferred. If a simpler bucket-based classification is sufficient, `NTILE()` could be used.
- Both solutions will allow ABC Corp to efficiently identify the top 5 percentile of fraudulent claims and ensure that experienced claim adjusters handle these high-risk cases.

### Data Analysis

Let's analyze the result of the SQL query to understand the distribution of the top 5 percentile of fraudulent claims by state.

**State-wise Distribution:**
- **California (CA):** 5 claims
- **Florida (FL):** 5 claims
- **New York (NY):** 5 claims
- **Texas (TX):** 6 claims

#### Analysis:

1. **Fraud Score Distribution:**
   - The fraud scores in the top 5 percentile are consistently high across all states, indicating that the top percentile claims are indeed high-risk.
   - For California, the fraud scores range from 0.948 to 0.988.
   - For Florida, the fraud scores range from 0.923 to 0.988.
   - For New York, the fraud scores range from 0.969 to 0.982.
   - For Texas, the fraud scores range from 0.960 to 0.999.

2. **Claim Cost Insights:**
   - Claim costs vary significantly within each state. For example, in California, claim costs range from $1,080 to $4,224.
   - In Florida, claim costs range from $1,419 to $2,581.
   - In New York, claim costs range from $2,994 to $4,903.
   - In Texas, claim costs range from $1,407 to $4,950.

3. **State-wise Comparison:**
   - **California and New York** have relatively higher fraud scores compared to Florida.
   - **Texas** shows the highest fraud score of 0.999, indicating a very high-risk claim in this state.

4. **Coverage:**
   - The dataset shows a good spread of top 5 percentile claims across different states, with a fairly balanced number of claims from each state except for Texas, which has one more claim than the others.

### Comment

The analysis of the top 5 percentile fraudulent claims reveals that ABC Corp's model is identifying high-risk claims effectively across states. The consistency of high fraud scores within the top 5 percentile confirms that the selection is robust and aligns with the goal of targeting the most potentially fraudulent claims. 

**Key Takeaways:**
- **Fraud Scores:** High fraud scores across the board indicate that the model's predictive capability is strong, as it consistently ranks the most suspicious claims at the top.
- **Claim Costs:** Significant variation in claim costs, combined with high fraud scores, suggests that both high-value and lower-value claims can be highly fraudulent. This variability should be considered when allocating resources to claim adjusters.
- **State Discrepancies:** The data suggests that the fraud risk may vary by state, which could be due to different claim patterns or varying levels of fraud in different regions. ABC Corp may want to investigate if certain states have inherent risk factors contributing to higher fraud scores.

Overall, the provided SQL solution is effective for identifying the top 5 percentile of claims based on fraud scores, ensuring that the highest-risk claims are prioritized for review by experienced adjusters.
