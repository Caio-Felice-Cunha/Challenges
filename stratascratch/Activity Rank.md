## Challenge link: https://platform.stratascratch.com/coding/10351-activity-rank?code_type=3

# Challenge: Activity Rank

![image](https://github.com/user-attachments/assets/5cf24dec-6f1e-4762-9f3b-ff84ec04df49)


Find the email activity rank for each user. Email activity rank is defined by the total number of emails sent. The user with the highest number of emails sent will have a rank of 1, and so on. Output the user, total emails, and their activity rank. Order records by the total emails in descending order. Sort users with the same number of emails in alphabetical order.
In your rankings, return a unique value (i.e., a unique rank) even if multiple users have the same number of emails. For tie breaker use alphabetical order of the user usernames.

![image](https://github.com/user-attachments/assets/bbf2cf86-d2d3-47b1-81a3-098a51673889)

# Answer:

``` sql
SELECT 
    from_user
    , COUNT(*) AS Emails_Sent
    , ROW_NUMBER()
        OVER( 
            ORDER BY COUNT(*) DESC
            , from_user ASC)
FROM 
    google_gmail_emails
GROUP BY 
    from_user
ORDER BY
    COUNT(*) DESC
    , from_user ASC;
```

![image](https://github.com/user-attachments/assets/bb34dad4-84d6-422e-8507-6b3e87ddbc52)


## Explanation:
### Challenge Breakdown: Activity Rank

The challenge requires determining the rank of users based on the total number of emails they have sent, with ties broken by alphabetical order. The solution is given in SQL, and here's a detailed breakdown of how it works:

### Problem Breakdown

1. **Identify User Activity**: Count the total number of emails sent by each user.
2. **Rank the Users**: Rank users based on the total emails sent, with the highest number ranked first.
3. **Handle Ties**: If two or more users have sent the same number of emails, rank them alphabetically by their username.
4. **Return Data**: Output each user’s username, total emails sent, and their rank.

### Code Explanation

```sql
SELECT 
    from_user,                          -- Selecting the username
    COUNT(*) AS Emails_Sent,            -- Counting the total emails sent by each user
    ROW_NUMBER()                        -- Assigning a unique rank to each user
        OVER( 
            ORDER BY COUNT(*) DESC,     -- Ordering users first by total emails sent (highest first)
            from_user ASC               -- For users with the same email count, ordering alphabetically by username
        ) AS rank                       -- Naming the resulting column as 'rank'
FROM 
    google_gmail_emails                 -- Indicating the table from which to pull the data
GROUP BY 
    from_user                           -- Grouping the results by username to calculate the total emails for each user
ORDER BY
    Emails_Sent DESC,                   -- Ordering the final output by the number of emails sent in descending order
    from_user ASC;                      -- Breaking ties by ordering alphabetically by username
```

### Step-by-Step Breakdown

1. **FROM Clause**: 
   - The `google_gmail_emails` table is the source of the data. It contains email records where each row likely represents an individual email, with one of the columns (`from_user`) containing the sender's username.

2. **GROUP BY Clause**:
   - `GROUP BY from_user` is used to aggregate the data by each user. This means that all rows with the same `from_user` will be grouped together, allowing us to calculate the total emails sent by each user.

3. **COUNT(*)**:
   - `COUNT(*) AS Emails_Sent` counts the total number of emails sent by each user (i.e., the number of rows in each group formed by `GROUP BY`).

4. **ROW_NUMBER()**:
   - `ROW_NUMBER()` is a window function that assigns a unique sequential integer to rows within a result set. The numbering is defined by the `ORDER BY` clause within the `OVER` function.
   - `ORDER BY COUNT(*) DESC, from_user ASC` inside `OVER()`:
     - First, it orders users by the total number of emails sent (`COUNT(*) DESC`). The user who sent the most emails gets the highest rank.
     - For users with the same email count, the `from_user ASC` ensures that they are ranked alphabetically.
   - The result of `ROW_NUMBER()` is the rank of the user, which is unique even when users have the same number of emails.

5. **ORDER BY Clause**:
   - Finally, the `ORDER BY Emails_Sent DESC, from_user ASC` ensures the output is sorted first by the total number of emails sent and then alphabetically by username.

### Alternative Solution

While the provided solution is effective, there's an alternative way to achieve the same results using the `DENSE_RANK()` function. This function also ranks rows, but unlike `ROW_NUMBER()`, it assigns the same rank to users with the same number of emails.

#### Alternative Code:

```sql
SELECT 
    from_user,
    COUNT(*) AS Emails_Sent,
    DENSE_RANK() 
        OVER(
            ORDER BY COUNT(*) DESC, 
            from_user ASC
        ) AS rank
FROM 
    google_gmail_emails
GROUP BY 
    from_user
ORDER BY 
    Emails_Sent DESC,
    from_user ASC;
```

#### Key Differences:
- **DENSE_RANK()**: This function assigns the same rank to users with the same number of emails. For example, if two users have sent the same number of emails and are tied for rank 1, both will be ranked 1, and the next user will be ranked 2.
- **ROW_NUMBER()**: Ensures that even in the case of ties, each user gets a unique rank, which is what the challenge specifically requires. Therefore, while `DENSE_RANK()` is an alternative, it does not fully meet the challenge's requirements, making `ROW_NUMBER()` the more appropriate choice.

### Conclusion

The given SQL solution effectively calculates the email activity rank using `ROW_NUMBER()`. This approach ensures each user gets a unique rank, even when there are ties, by using alphabetical ordering as a tiebreaker. The alternative solution using `DENSE_RANK()` offers a different ranking strategy, but it doesn't fully satisfy the challenge's requirement for unique ranking in the case of ties.
