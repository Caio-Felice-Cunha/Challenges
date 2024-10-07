## Challenge link: https://datalemur.com/questions/signup-confirmation-rate

# Challenge: Signup Activation Rate [TikTok SQL Interview Question]

New TikTok users sign up with their emails. They confirmed their signup by replying to the text confirmation to activate their accounts. Users may receive multiple text messages for account confirmation until they have confirmed their new account.

A senior analyst is interested to know the activation rate of specified users in the emails table. Write a query to find the activation rate. Round the percentage to 2 decimal places.

Definitions:

emails table contain the information of user signup details.
texts table contains the users' activation information.
Assumptions:

The analyst is interested in the activation rate of specific users in the emails table, which may not include all users that could potentially be found in the texts table.
For example, user 123 in the emails table may not be in the texts table and vice versa.
Effective April 4th 2023, we added an assumption to the question to provide additional clarity.

![image](https://github.com/user-attachments/assets/0cf9a257-de42-4144-a269-f5878a53f7da) ![image](https://github.com/user-attachments/assets/456a8066-07ec-4a44-8095-615a22c72eb3)


# Answer:

``` sql
SELECT 
  ROUND(
    COUNT(texts.signup_action) 
    FILTER (WHERE texts.signup_action = 'Confirmed')::numeric
  /
    COUNT(texts.email_id), 
    2
  ) AS Rate
FROM 
  emails
FULL OUTER JOIN 
  texts
ON
  emails.email_id = texts.email_id;
```
![image](https://github.com/user-attachments/assets/5e8ac18e-5766-421b-92dc-531b3f91419c)


## Explanation:
To solve the **Signup Activation Rate** challenge, let's break down the provided solution and discuss how it calculates the activation rate. Additionally, I’ll propose an alternative solution for comparison.

### Problem Recap
You need to calculate the activation rate for specified users in the `emails` table by determining how many of these users confirmed their accounts via a text message, based on the data in the `texts` table.

**Tables:**
- `emails`: Contains user signup details.
- `texts`: Contains information about users’ text message actions, including account confirmations.

### Provided Solution Breakdown

```sql
SELECT 
  ROUND(
    COUNT(texts.signup_action) 
    FILTER (WHERE texts.signup_action = 'Confirmed')::numeric
  /
    COUNT(texts.email_id), 
    2
  ) AS Rate
FROM 
  emails
FULL OUTER JOIN 
  texts
ON
  emails.email_id = texts.email_id;
```

#### Step-by-Step Explanation:

1. **Join the Tables:**
   ```sql
   FROM emails
   FULL OUTER JOIN texts
   ON emails.email_id = texts.email_id;
   ```
   - **FULL OUTER JOIN** is used here to combine the `emails` and `texts` tables based on the `email_id` column. This join ensures that all users from both tables are included in the result, even if there’s no match between the two tables.

2. **Count the Confirmed Activations:**
   ```sql
   COUNT(texts.signup_action) 
   FILTER (WHERE texts.signup_action = 'Confirmed')::numeric
   ```
   - This line counts the number of `signup_action` entries in the `texts` table where the action is 'Confirmed'. 
   - The `FILTER` clause ensures that only rows where the `signup_action` is 'Confirmed' are considered.
   - The result is then cast to a numeric type to prepare for division.

3. **Count All Texts:**
   ```sql
   COUNT(texts.email_id)
   ```
   - This counts the total number of entries in the `texts` table that have a corresponding `email_id`, which reflects the total number of text actions (including confirmations and any other actions).

4. **Calculate the Activation Rate:**
   ```sql
   ROUND(
    COUNT(texts.signup_action) 
    FILTER (WHERE texts.signup_action = 'Confirmed')::numeric
   /
    COUNT(texts.email_id), 
    2
   ) AS Rate
   ```
   - The division of the count of confirmed actions by the total count of text actions gives us the activation rate.
   - The `ROUND(..., 2)` function is used to round the result to 2 decimal places.
   - The final result is labeled as `Rate`.

### Assumptions Made in the Solution:
- The solution assumes that the only relevant users are those present in the `emails` table.
- Users not present in the `emails` table but present in the `texts` table do not affect the activation rate calculation.

### Alternative Solution

Another way to approach the problem is to focus only on the users from the `emails` table who have confirmed their signup. This can be done using an **INNER JOIN** and conditional aggregation. Here’s how:

```sql
SELECT 
  ROUND(
    COUNT(DISTINCT texts.email_id) * 1.0
  /
    COUNT(DISTINCT emails.email_id), 
    2
  ) AS Rate
FROM 
  emails
LEFT JOIN 
  texts
ON 
  emails.email_id = texts.email_id
AND 
  texts.signup_action = 'Confirmed';
```

#### Alternative Solution Breakdown:

1. **Join the Tables with Condition:**
   ```sql
   LEFT JOIN texts ON emails.email_id = texts.email_id AND texts.signup_action = 'Confirmed';
   ```
   - A **LEFT JOIN** is used here to include all users from the `emails` table, along with their corresponding entries in the `texts` table where the `signup_action` is 'Confirmed'. 
   - Users without a confirmed signup will have a `NULL` in the `texts` columns.

2. **Count Unique Confirmed Users:**
   ```sql
   COUNT(DISTINCT texts.email_id) * 1.0
   ```
   - `COUNT(DISTINCT texts.email_id)` counts the number of unique users in the `texts` table who have confirmed their signup.
   - Multiplying by `1.0` ensures the result is treated as a numeric type for the division.

3. **Count Total Unique Users:**
   ```sql
   COUNT(DISTINCT emails.email_id)
   ```
   - `COUNT(DISTINCT emails.email_id)` counts the total number of unique users in the `emails` table.

4. **Calculate and Round the Activation Rate:**
   ```sql
   ROUND(
     COUNT(DISTINCT texts.email_id) * 1.0 / COUNT(DISTINCT emails.email_id), 
     2
   ) AS Rate
   ```
   - The ratio of confirmed users to total users is calculated and rounded to 2 decimal places.

### Comparison of Solutions

- **Original Solution:** Uses a FULL OUTER JOIN and calculates the rate by counting all actions, confirmed or not. This approach is comprehensive but may include rows from the `texts` table that are not in the `emails` table.
  
- **Alternative Solution:** Uses a LEFT JOIN, focusing on users in the `emails` table and their confirmed actions only. This solution directly aligns with the assumption that we are only interested in users specified in the `emails` table.

Both solutions are valid, but the choice depends on the specific business requirements. The alternative solution is often preferred when focusing strictly on the `emails` table users, as it avoids including irrelevant users from the `texts` table.
