## Challenge link: https://platform.stratascratch.com/coding/10166-reviews-of-hotel-arena?code_type=3

# Challenge: Reviews of Hotel Arena
![image](https://github.com/user-attachments/assets/0b534f2f-c73c-46e0-b52e-15ce6453f513)

Find the number of rows for each review score earned by 'Hotel Arena'. Output the hotel name (which should be 'Hotel Arena'), review score along with the corresponding number of rows with that score for the specified hotel.

![image](https://github.com/user-attachments/assets/5c7f3500-0597-4125-9459-83d6a84ddbee)


# Answer:

``` sql
SELECT 
    hotel_name
    , reviewer_score
    , COUNT(reviewer_score)
FROM
    hotel_reviews
WHERE 
    hotel_name = 'Hotel Arena'
GROUP BY 
    hotel_name
    , reviewer_score
ORDER BY 
    reviewer_score DESC
;
``` 
![image](https://github.com/user-attachments/assets/21c57f33-6fa1-4b93-9cf7-f6c26571c7de)


## Explanation:
Sure! Let's break down the SQL query provided for the "Reviews of Hotel Arena" challenge and also explore an alternative approach.

### Detailed Breakdown of the Code

**Query:**
```sql
SELECT 
    hotel_name,
    reviewer_score,
    COUNT(reviewer_score)
FROM
    hotel_reviews
WHERE 
    hotel_name = 'Hotel Arena'
GROUP BY 
    hotel_name,
    reviewer_score
ORDER BY 
    reviewer_score DESC;
```

1. **SELECT Clause:**
   ```sql
   SELECT 
       hotel_name,
       reviewer_score,
       COUNT(reviewer_score)
   ```
   - `hotel_name`: This column retrieves the name of the hotel, which is 'Hotel Arena' in this case.
   - `reviewer_score`: This column retrieves the score given by reviewers.
   - `COUNT(reviewer_score)`: This function counts the number of occurrences of each `reviewer_score`. This count corresponds to how many reviews were given with that particular score.

2. **FROM Clause:**
   ```sql
   FROM hotel_reviews
   ```
   - Specifies the table `hotel_reviews` from which to retrieve the data.

3. **WHERE Clause:**
   ```sql
   WHERE hotel_name = 'Hotel Arena'
   ```
   - Filters the rows to include only those where the `hotel_name` is 'Hotel Arena'. This ensures that the query focuses solely on reviews for this specific hotel.

4. **GROUP BY Clause:**
   ```sql
   GROUP BY hotel_name, reviewer_score
   ```
   - Groups the results by `hotel_name` and `reviewer_score`. This means that for each combination of `hotel_name` and `reviewer_score`, the `COUNT(reviewer_score)` function is applied, resulting in the number of reviews with that score for 'Hotel Arena'.

5. **ORDER BY Clause:**
   ```sql
   ORDER BY reviewer_score DESC
   ```
   - Orders the results by `reviewer_score` in descending order. This means the highest scores appear first, followed by lower scores.

### Result:
The result set lists each review score for 'Hotel Arena' and the number of times that score was given, sorted from the highest to the lowest score. For example:
- 9.6 is given twice,
- 8.8 is given twice, and so on.

### Alternative Solution

An alternative approach to achieve the same result would be to use a subquery or Common Table Expression (CTE) for additional flexibility. For instance, using a CTE:

**Alternative Query:**
```sql
WITH ArenaReviews AS (
    SELECT 
        reviewer_score
    FROM 
        hotel_reviews
    WHERE 
        hotel_name = 'Hotel Arena'
)
SELECT 
    'Hotel Arena' AS hotel_name,
    reviewer_score,
    COUNT(*) AS review_count
FROM 
    ArenaReviews
GROUP BY 
    reviewer_score
ORDER BY 
    reviewer_score DESC;
```

**Explanation of Alternative Query:**

1. **WITH Clause (CTE):**
   ```sql
   WITH ArenaReviews AS (
       SELECT 
           reviewer_score
       FROM 
           hotel_reviews
       WHERE 
           hotel_name = 'Hotel Arena'
   )
   ```
   - This creates a temporary result set `ArenaReviews` that contains only the `reviewer_score` for 'Hotel Arena'. It simplifies the main query by pre-filtering the data.

2. **Main SELECT Statement:**
   ```sql
   SELECT 
       'Hotel Arena' AS hotel_name,
       reviewer_score,
       COUNT(*) AS review_count
   FROM 
       ArenaReviews
   GROUP BY 
       reviewer_score
   ORDER BY 
       reviewer_score DESC;
   ```
   - `'Hotel Arena' AS hotel_name`: Directly includes the hotel name in the final result set.
   - `COUNT(*) AS review_count`: Counts the number of occurrences of each score from the CTE.
   - Groups by `reviewer_score` and orders by `reviewer_score DESC`, just like in the original query.

This alternative approach is particularly useful if the query needs to be extended with additional filters or joins later on. It makes the main query cleaner and potentially easier to maintain.
