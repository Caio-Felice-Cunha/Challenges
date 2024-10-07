## Challenge link: https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=true

# Challenge: Challenges

Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

Input Format

The following tables contain challenge data:

Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.

![image](https://github.com/user-attachments/assets/1709459b-1249-4717-989c-f2f6f7c829a2)

Challenges: The challenge_id is the id of the challenge, and hacker_id is the id of the student who created the challenge.

![image](https://github.com/user-attachments/assets/39e80210-c22a-4fb0-96b6-ef5adb3c1bde)



# Answer:

``` sql
WITH Chal_Stud AS
    (SELECT 
        hackers.hacker_id
        , hackers.name
        , count(challenge_id) AS COUNT_CHAL
    FROM 
        hackers
    JOIN 
        challenges
        on hackers.hacker_id = challenges.hacker_id
    GROUP BY 
        hacker_id
        , name
    ORDER BY 
        COUNT_CHAL desc
        , hacker_id),
Max_Table AS
    (SELECT 
        max(COUNT_CHAL) AS MAX_CHAL 
    FROM 
        Chal_Stud),
Excluding_Table AS    
    (SELECT 
        Chal_Stud_a.hacker_id
        , Chal_Stud_a.name
        , Chal_Stud_a.COUNT_CHAL
    FROM 
        Chal_Stud AS Chal_Stud_a
        , Chal_Stud AS Chal_Stud_b
    WHERE 
        Chal_Stud_a.hacker_id <> Chal_Stud_b.hacker_id 
        AND Chal_Stud_a.COUNT_CHAL < (
                                    SELECT 
                                        MAX_CHAL 
                                    FROM 
                                        Max_Table) 
            AND Chal_Stud_a.COUNT_CHAL = Chal_Stud_b.COUNT_CHAL)
          
SELECT 
    hacker_id
    , name
    , COUNT_CHAL
FROM 
    Chal_Stud
WHERE 
    hacker_id NOT IN (
        SELECT 
            hacker_id 
        FROM 
            Excluding_Table
        );
```

### Result:

![image](https://github.com/user-attachments/assets/2048804a-7273-4b2c-bdec-66f2e71ff901)


## Explanation:
### Code Breakdown and Explanation

This SQL query is designed to retrieve the `hacker_id`, `name`, and the total number of challenges created by each student. The result is sorted by the total number of challenges in descending order. If more than one student created the same number of challenges and that number is less than the maximum number of challenges created, then those students are excluded from the final result.

#### 1. **Chal_Stud CTE (Common Table Expression)**

```sql
WITH Chal_Stud AS
    (SELECT 
        hackers.hacker_id,
        hackers.name,
        count(challenge_id) AS COUNT_CHAL
    FROM 
        hackers
    JOIN 
        challenges
        on hackers.hacker_id = challenges.hacker_id
    GROUP BY 
        hacker_id,
        name
    ORDER BY 
        COUNT_CHAL desc,
        hacker_id),
```

- **Purpose**: This part of the query creates a temporary result set (using a CTE) that joins the `hackers` and `challenges` tables. It groups the results by `hacker_id` and `name`, and counts the number of challenges created by each hacker.
- **Sorting**: The result is sorted by `COUNT_CHAL` in descending order, and by `hacker_id` as a secondary sort criterion to ensure consistent ordering.

#### 2. **Max_Table CTE**

```sql
Max_Table AS
    (SELECT 
        max(COUNT_CHAL) AS MAX_CHAL 
    FROM 
        Chal_Stud),
```

- **Purpose**: This CTE calculates the maximum number of challenges created by any single hacker. The result is stored in the `MAX_CHAL` column.

#### 3. **Excluding_Table CTE**

```sql
Excluding_Table AS    
    (SELECT 
        Chal_Stud_a.hacker_id,
        Chal_Stud_a.name,
        Chal_Stud_a.COUNT_CHAL
    FROM 
        Chal_Stud AS Chal_Stud_a,
        Chal_Stud AS Chal_Stud_b
    WHERE 
        Chal_Stud_a.hacker_id <> Chal_Stud_b.hacker_id 
        AND Chal_Stud_a.COUNT_CHAL < (
                                    SELECT 
                                        MAX_CHAL 
                                    FROM 
                                        Max_Table) 
        AND Chal_Stud_a.COUNT_CHAL = Chal_Stud_b.COUNT_CHAL)
```

- **Purpose**: This CTE identifies students who created the same number of challenges but not the maximum number. It compares each record in `Chal_Stud` with others to find students with identical `COUNT_CHAL` values that are less than the `MAX_CHAL`.
- **Filtering**: The condition `Chal_Stud_a.hacker_id <> Chal_Stud_b.hacker_id` ensures that only different students are compared.

#### 4. **Final Selection**

```sql
SELECT 
    hacker_id,
    name,
    COUNT_CHAL
FROM 
    Chal_Stud
WHERE 
    hacker_id NOT IN (
        SELECT 
            hacker_id 
        FROM 
            Excluding_Table
        );
```

- **Purpose**: This part of the query selects the final list of hackers from `Chal_Stud`, excluding those present in the `Excluding_Table`. The exclusion ensures that only students who created the maximum number of challenges or unique challenge counts are included in the final result.

### Data Analysis of the Result

The final result lists students who have either created the maximum number of challenges (which is 50 in this case) or have a unique number of challenges not shared by others. 

#### Summary of Results:
- **Max Count Group**: There are 12 students who have created 50 challenges each.
- **Unique Count Group**: The other students listed have created a unique number of challenges, different from others.

#### Insights:
- The highest number of challenges created by any student is 50, and multiple students share this maximum value.
- The unique challenge counts in the list range from 42 challenges down to 8 challenges.
- All students who created the same number of challenges as others, but less than 50, are excluded from the result.

### Comment

The solution effectively filters out students who do not meet the criteria of having either the maximum or a unique count of challenges. This ensures that the results reflect only the most significant contributions in terms of challenge creation.

### Further or Alternative Analysis

#### Alternative Analysis: Identify Students with Unique Challenge Counts

To explore a different aspect of the data, we could modify the query to identify only those students who have created a unique number of challenges (i.e., no other student has created the same number).

#### Modified Query:

```sql
WITH Chal_Stud AS
    (SELECT 
        hackers.hacker_id,
        hackers.name,
        count(challenge_id) AS COUNT_CHAL
    FROM 
        hackers
    JOIN 
        challenges
        on hackers.hacker_id = challenges.hacker_id
    GROUP BY 
        hacker_id,
        name),
Unique_Chal AS
    (SELECT 
        hacker_id,
        name,
        COUNT_CHAL
    FROM 
        Chal_Stud
    GROUP BY 
        COUNT_CHAL
    HAVING 
        COUNT(*) = 1)

SELECT 
    hacker_id,
    name,
    COUNT_CHAL
FROM 
    Unique_Chal
ORDER BY 
    COUNT_CHAL DESC,
    hacker_id;
```

### Explanation:

- **Unique_Chal CTE**: This CTE filters the `Chal_Stud` result to include only those students with a unique count of challenges.
- **Final Selection**: The final selection retrieves and orders the students based on their unique contribution.

### Conclusion

This alternative analysis allows for a different perspective on the data, focusing on students with unique challenge counts, which might be of interest when looking for standout individual contributions.
