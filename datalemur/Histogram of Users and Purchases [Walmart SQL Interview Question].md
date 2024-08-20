## Challenge link: https://datalemur.com/questions/histogram-users-purchases

# Challenge: Histogram of Users and Purchases [Walmart SQL Interview Question]

This is the same question as problem #13 in the SQL Chapter of Ace the Data Science Interview!

Assume you're given a table on Walmart user transactions. Based on their most recent transaction date, write a query that retrieve the users along with the number of products they bought.

Output the user's most recent transaction date, user ID, and the number of products, sorted in chronological order by the transaction date.

Starting from November 10th, 2022, the official solution was updated, and the expected output of transaction date, number of users, and number of products was changed to the current expected output.

![image](https://github.com/user-attachments/assets/7df096a9-19e5-4038-82ef-edf112c985af)


# Answer:

``` sql
SELECT
  transaction_date
  , user_id
  , COUNT(Purchase_Order) Item_Purchased
FROM(
  SELECT 
     transaction_date
     , user_id
     , product_id
     , RANK() OVER(
            PARTITION BY user_id
            ORDER BY transaction_date DESC) AS Purchase_Order
  FROM 
    user_transactions) AS Rank_Table
WHERE
  Purchase_Order = 1
GROUP BY
  transaction_date
  , user_id
ORDER BY
  transaction_date;
```

![image](https://github.com/user-attachments/assets/470a7aab-e3b3-4f0f-b2e2-dc00aa7711aa)

## Explanation:

Let's break down the SQL query provided for the Walmart user transactions challenge and analyze the results. Additionally, I'll provide an alternative solution and a data analysis.

### Problem Breakdown

We have a `user_transactions` table that records details of transactions made by users. The goal is to retrieve each user's most recent transaction date, their user ID, and the number of products they purchased in that transaction. The result should be sorted in chronological order by the transaction date.

### Table Structure

- **product_id**: The ID of the product purchased.
- **user_id**: The ID of the user making the purchase.
- **spend**: The amount spent on the transaction.
- **transaction_date**: The date and time when the transaction occurred.

### Step-by-Step Explanation of the Provided Solution

Here's the provided query:

```sql
SELECT
  transaction_date,
  user_id,
  COUNT(Purchase_Order) AS Item_Purchased
FROM (
  SELECT 
    transaction_date,
    user_id,
    product_id,
    RANK() OVER (
      PARTITION BY user_id
      ORDER BY transaction_date DESC
    ) AS Purchase_Order
  FROM 
    user_transactions
) AS Rank_Table
WHERE
  Purchase_Order = 1
GROUP BY
  transaction_date,
  user_id
ORDER BY
  transaction_date;
```

#### Step 1: Inner Query

```sql
SELECT 
  transaction_date,
  user_id,
  product_id,
  RANK() OVER (
    PARTITION BY user_id
    ORDER BY transaction_date DESC
  ) AS Purchase_Order
FROM 
  user_transactions
```

- **RANK()**: The `RANK()` window function assigns a rank to each row within a partition of a result set (in this case, by `user_id`), ordering by `transaction_date` in descending order.
- **PARTITION BY user_id**: This divides the dataset into partitions where each partition consists of rows for a single `user_id`.
- **ORDER BY transaction_date DESC**: Within each partition, rows are ordered by `transaction_date` in descending order, meaning the most recent transaction will have the rank of 1.

This inner query creates a temporary table with columns `transaction_date`, `user_id`, `product_id`, and `Purchase_Order` (which indicates the ranking of the transaction date for each user).

#### Step 2: Filtering the Most Recent Transaction

```sql
WHERE
  Purchase_Order = 1
```

- This `WHERE` clause filters out all rows except those where `Purchase_Order` equals 1, i.e., the most recent transaction for each user.

#### Step 3: Aggregating Results

```sql
SELECT
  transaction_date,
  user_id,
  COUNT(Purchase_Order) AS Item_Purchased
```

- **COUNT(Purchase_Order)**: Since we only have rows where `Purchase_Order = 1`, counting these gives the number of products purchased in that transaction.

#### Step 4: Grouping and Ordering

```sql
GROUP BY
  transaction_date,
  user_id
ORDER BY
  transaction_date;
```

- **GROUP BY**: Groups the result by `transaction_date` and `user_id`, so we get the number of products bought in the most recent transaction for each user.
- **ORDER BY transaction_date**: Sorts the final output by `transaction_date` in chronological order.

### Alternative Solution

Another approach could be using the `ROW_NUMBER()` function, which is similar to `RANK()` but ensures no ties (i.e., no duplicate ranks for the same `user_id` and `transaction_date` combination):

```sql
WITH Ranked_Transactions AS (
  SELECT
    transaction_date,
    user_id,
    product_id,
    ROW_NUMBER() OVER (
      PARTITION BY user_id
      ORDER BY transaction_date DESC
    ) AS Row_Num
  FROM
    user_transactions
)
SELECT
  transaction_date,
  user_id,
  COUNT(product_id) AS Item_Purchased
FROM
  Ranked_Transactions
WHERE
  Row_Num = 1
GROUP BY
  transaction_date,
  user_id
ORDER BY
  transaction_date;
```

- **ROW_NUMBER()**: Similar to `RANK()` but guarantees no ties, which might be useful if we want a strict sequence without duplicate rankings.
- The rest of the logic follows similarly to the original solution.

### Data Analysis

Both solutions are designed to retrieve the most recent transaction per user and the number of products purchased during that transaction. 

For example, consider the following `user_transactions` data:

| product_id | user_id | spend | transaction_date    |
|------------|---------|-------|---------------------|
| 3673       | 123     | 68.90 | 2022-07-08 12:00:00 |
| 9623       | 123     | 274.10| 2022-07-08 12:00:00 |
| 1467       | 115     | 19.90 | 2022-07-08 12:00:00 |
| 2513       | 159     | 25.00 | 2022-07-08 12:00:00 |
| 1452       | 159     | 74.50 | 2022-07-10 12:00:00 |

### Expected Output

| transaction_date    | user_id | Item_Purchased |
|---------------------|---------|----------------|
| 2022-07-08 12:00:00 | 115     | 1              |
| 2022-07-08 12:00:00 | 123     | 2              |
| 2022-07-10 12:00:00 | 159     | 1              |

**Comment:**

- **Chronological Order**: The results are ordered by transaction date, fulfilling the requirement of chronological sorting.
- **Accuracy**: The query ensures that we capture the most recent transaction for each user, ensuring no duplicate rows for the same user.

### Conclusion

Both solutions are effective, but the original query using `RANK()` might produce duplicate ranks if users have multiple transactions on the same date and time. If strict uniqueness is required, the alternative solution with `ROW_NUMBER()` is preferred.

This solution is efficient and provides a clear breakdown of user purchase behavior, useful for business insights such as understanding purchasing patterns and peak transaction times.
