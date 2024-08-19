## Challenge link: https://www.hackerrank.com/challenges/full-score/problem?isFullScreen=true

# Challenge: Top Competitors

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.

Input Format

The following tables contain contest data:

Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker.

![image](https://github.com/user-attachments/assets/7f0db93f-d354-401c-aee6-d50d225c4144) ![image](https://github.com/user-attachments/assets/68a6b245-a93f-4f34-b9e8-d777e80b8a32)


Difficulty: The difficult_level is the level of difficulty of the challenge, and score is the score of the challenge for the difficulty level.

![image](https://github.com/user-attachments/assets/00e33410-29a5-4298-80c0-01c2ddf35f07) ![image](https://github.com/user-attachments/assets/1ab94e31-c06c-4e78-b77c-09618a957c23)


Challenges: The challenge_id is the id of the challenge, the hacker_id is the id of the hacker who created the challenge, and difficulty_level is the level of difficulty of the challenge.

![image](https://github.com/user-attachments/assets/e9046e9a-e82a-4c71-9d58-6fd9d5c8651d) ![image](https://github.com/user-attachments/assets/e8df626d-e5b9-4486-9cf0-9b4e2ba7c53f)


Submissions: The submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission, challenge_id is the id of the challenge that the submission belongs to, and score is the score of the submission.

![image](https://github.com/user-attachments/assets/bb34cca2-66db-4c24-905d-be0c073a2bd5) ![image](https://github.com/user-attachments/assets/41eee275-c16d-4956-b557-8b7f82a964a7)



# Answer:

``` sql
SELECT 
    Hackers.hacker_id
    , Hackers.name
FROM 
    Hackers
JOIN 
    Submissions 
    ON 
        Hackers.hacker_id = Submissions.hacker_id
JOIN 
    Challenges 
    ON 
        Submissions.challenge_id = Challenges.challenge_id
JOIN 
    Difficulty
    ON 
        Challenges.difficulty_level = Difficulty.difficulty_level
WHERE 
    Submissions.score = Difficulty.score
GROUP BY 
    Hackers.hacker_id
    , Hackers.name
HAVING 
    count(*)>1
ORDER BY 
    COUNT(*) DESC
    , Hackers.hacker_id ASC;

```



## Explanation:
To solve the challenge of finding the hackers who achieved full scores in more than one challenge, we need to carefully analyze the structure of the tables involved and write a SQL query that meets the requirements. Below is a detailed breakdown of the provided solution, followed by an alternative approach, data analysis, and commentary.

### Breakdown of the Provided Solution

**1. Understanding the Tables:**
- **Hackers:** Contains `hacker_id` and `name`.
- **Challenges:** Contains `challenge_id`, `hacker_id`, and `difficulty_level`.
- **Submissions:** Contains `submission_id`, `hacker_id`, `challenge_id`, and `score`.

**2. Required Output:**
- We need to find hackers who achieved full scores in more than one challenge.
- The output should be ordered by the number of full scores (in descending order). If two hackers have the same number of full scores, we order them by `hacker_id` in ascending order.

**3. SQL Query Explanation:**

```sql
SELECT 
    Hackers.hacker_id,
    Hackers.name
FROM 
    Hackers
JOIN 
    Submissions 
    ON Hackers.hacker_id = Submissions.hacker_id
JOIN 
    Challenges 
    ON Submissions.challenge_id = Challenges.challenge_id
JOIN 
    Difficulty
    ON Challenges.difficulty_level = Difficulty.difficulty_level
WHERE 
    Submissions.score = Difficulty.score
GROUP BY 
    Hackers.hacker_id, Hackers.name
HAVING 
    COUNT(*) > 1
ORDER BY 
    COUNT(*) DESC,
    Hackers.hacker_id ASC;
```

**Step-by-Step Explanation:**

- **JOIN Operations:**
  - We join the **Hackers** table with the **Submissions** table on `hacker_id` to find the submissions made by each hacker.
  - Next, we join the **Submissions** table with the **Challenges** table on `challenge_id` to link submissions to the specific challenges.
  - Lastly, we join the **Challenges** table with the **Difficulty** table to get the difficulty level and the corresponding full score for each challenge.

- **WHERE Clause:**
  - `WHERE Submissions.score = Difficulty.score`: This condition filters out the submissions where the hacker's score matches the full score for that challenge.

- **GROUP BY Clause:**
  - `GROUP BY Hackers.hacker_id, Hackers.name`: We group the results by `hacker_id` and `name` to calculate the count of full scores for each hacker.

- **HAVING Clause:**
  - `HAVING COUNT(*) > 1`: This ensures that we only select hackers who achieved full scores in more than one challenge.

- **ORDER BY Clause:**
  - `ORDER BY COUNT(*) DESC, Hackers.hacker_id ASC`: First, we sort by the count of full scores in descending order. If there is a tie, we sort by `hacker_id` in ascending order.

### Data Analysis of the Result

**1. Analysis:**
- The result shows a list of hackers who have scored full points in more than one challenge.
- The hackers are ordered primarily by the number of challenges in which they scored full marks.
- In case of a tie in the number of full scores, the hackers are ordered by their `hacker_id`.

**2. Observations:**
- **Phillip (27232)** and **Willie (28614)** top the leaderboard, meaning they likely achieved full scores in the highest number of challenges.
- There are multiple hackers with the same first name but different `hacker_id`s, indicating that the leaderboard has unique individuals.

### Alternative Solution

While the provided solution is correct, there's an alternative method to achieve the same result by using a subquery.

**Alternative Query:**

```sql
SELECT 
    hacker_id,
    name
FROM 
    (
        SELECT 
            Hackers.hacker_id,
            Hackers.name,
            COUNT(*) AS full_scores
        FROM 
            Hackers
        JOIN 
            Submissions 
            ON Hackers.hacker_id = Submissions.hacker_id
        JOIN 
            Challenges 
            ON Submissions.challenge_id = Challenges.challenge_id
        JOIN 
            Difficulty
            ON Challenges.difficulty_level = Difficulty.difficulty_level
        WHERE 
            Submissions.score = Difficulty.score
        GROUP BY 
            Hackers.hacker_id, Hackers.name
        HAVING 
            COUNT(*) > 1
    ) AS full_score_counts
ORDER BY 
    full_scores DESC, 
    hacker_id ASC;
```

**Explanation:**
- The inner query calculates the number of full scores for each hacker using the `COUNT(*)` function.
- The outer query then selects only the `hacker_id`, `name`, and orders the results by `full_scores` and `hacker_id`.

### Commentary

Both the original and alternative solutions effectively solve the challenge by identifying hackers who have demonstrated consistent performance across multiple challenges. The data reveals that some hackers, like Phillip and Willie, are top performers, indicating their proficiency in coding contests.

The solution could be further enhanced by providing additional insights, such as the exact number of full scores achieved by each hacker or breaking down the performance by challenge difficulty levels.

### Conclusion

The provided SQL query is well-crafted to meet the challenge's requirements, efficiently identifying and ordering hackers who have achieved full scores in multiple challenges. The alternative approach offers a slightly different structure but arrives at the same results. Both methods are valid, and the choice between them depends on personal or project-specific preferences.
