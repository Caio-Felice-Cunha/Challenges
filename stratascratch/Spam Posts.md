## Challenge link: https://platform.stratascratch.com/coding/10134-spam-posts?code_type=3

# Challenge: Spam Posts

![image](https://github.com/user-attachments/assets/001f496c-6a16-48cd-993d-3554949a296b)

Calculate the percentage of spam posts in all viewed posts by day. A post is considered a spam if a string "spam" is inside keywords of the post. Note that the facebook_posts table stores all posts posted by users. The facebook_post_views table is an action table denoting if a user has viewed a post.

![image](https://github.com/user-attachments/assets/49455748-8393-4762-a7be-eb8a67f11808)

![image](https://github.com/user-attachments/assets/2049ecf3-5949-4b77-8f0b-6d6c6362f0ec)


# Answer:

``` sql
SELECT
  facebook_posts.post_date
  , (SUM(CASE 
      WHEN facebook_posts.post_keywords LIKE '%spam%'
        AND facebook_post_views.viewer_id IS NOT NULL
      THEN 1 
      ELSE 0 
  END)
  /
  SUM(CASE 
      WHEN facebook_post_views.viewer_id IS NOT NULL
      THEN 1 
      ELSE 0 
  END)
  )*100 AS Spam_Share
FROM
  facebook_posts
LEFT JOIN
  facebook_post_views
  ON facebook_posts.post_id = facebook_post_views.post_id
GROUP BY
  facebook_posts.post_date;
```

![image](https://github.com/user-attachments/assets/3978d3e2-b550-49c3-8f0c-adc3159441d2)


## Explanation:
### Code Breakdown and Detailed Explanation

The given SQL query calculates the percentage of spam posts in all viewed posts by day. Let's break down the query step by step:

#### 1. `SELECT facebook_posts.post_date`
- **Purpose**: This selects the date when the post was created from the `facebook_posts` table.
- **Explanation**: `post_date` is the date on which the post was made. We need this to group our results by day.

#### 2. `, (SUM(CASE WHEN facebook_posts.post_keywords LIKE '%spam%' AND facebook_post_views.viewer_id IS NOT NULL THEN 1 ELSE 0 END)`
- **Purpose**: This part of the query calculates the number of spam posts that were viewed on a given day.
- **Explanation**:
  - **`CASE WHEN`**: The `CASE` statement is used to evaluate conditions and return a specific value based on those conditions.
  - **Condition**: `facebook_posts.post_keywords LIKE '%spam%'` checks if the post contains the keyword "spam". This determines if the post is considered spam.
  - **Additional Condition**: `facebook_post_views.viewer_id IS NOT NULL` checks if the post has been viewed (i.e., there is a corresponding viewer for the post).
  - **Result**: If both conditions are true (the post is spam and has been viewed), then `1` is added to the sum; otherwise, `0` is added.

#### 3. `/ SUM(CASE WHEN facebook_post_views.viewer_id IS NOT NULL THEN 1 ELSE 0 END)) * 100 AS Spam_Share`
- **Purpose**: This calculates the percentage of spam posts out of all viewed posts for each day.
- **Explanation**:
  - **Numerator**: The total number of spam posts that were viewed.
  - **Denominator**: The total number of posts that were viewed (regardless of whether they are spam or not).
  - **Multiplication by 100**: To convert the ratio into a percentage.
  - **Alias `Spam_Share`**: This represents the percentage of viewed posts that are spam for each day.

#### 4. `FROM facebook_posts`
- **Purpose**: This specifies the main table from which we are selecting data, i.e., `facebook_posts`.

#### 5. `LEFT JOIN facebook_post_views ON facebook_posts.post_id = facebook_post_views.post_id`
- **Purpose**: This joins the `facebook_posts` table with the `facebook_post_views` table to link posts with their views.
- **Explanation**:
  - **`LEFT JOIN`**: A `LEFT JOIN` ensures that all posts are included, even if they haven't been viewed. The `facebook_post_views` table contains the information about which posts have been viewed by users.
  - **`ON` Clause**: The join condition ensures that we are matching each post with its corresponding views using `post_id`.

#### 6. `GROUP BY facebook_posts.post_date`
- **Purpose**: This groups the results by the `post_date` so that the percentage is calculated for each day separately.

### Further or Alternative Analysis

#### Alternative Analysis: Consider Posts Without Views

If you want to calculate the percentage of spam posts including those that have never been viewed, you could modify the query by removing the condition `facebook_post_views.viewer_id IS NOT NULL`. Here's how the code could be modified:

```sql
SELECT
  facebook_posts.post_date,
  (SUM(CASE 
      WHEN facebook_posts.post_keywords LIKE '%spam%'
      THEN 1 
      ELSE 0 
  END)
  /
  COUNT(facebook_posts.post_id)
  ) * 100 AS Spam_Share
FROM
  facebook_posts
LEFT JOIN
  facebook_post_views
  ON facebook_posts.post_id = facebook_post_views.post_id
GROUP BY
  facebook_posts.post_date;
```

- **Explanation**: 
  - **Numerator**: Counts the total number of spam posts, regardless of whether they were viewed or not.
  - **Denominator**: The total number of posts, including those that were never viewed. This provides a broader understanding of how much spam content is being generated, not just viewed.

### Data Analysis of the Result

#### Example Output:
| post_date  | Spam_Share |
|------------|------------|
| 2019-01-01 | 100        |
| 2019-01-02 | 50         |

#### Analysis:
- **2019-01-01**: 100% of the posts that were viewed on this day were spam. This suggests that all posts viewed on this day contained the keyword "spam".
- **2019-01-02**: 50% of the viewed posts on this day were spam, indicating a mix of spam and non-spam content being viewed.

#### Comment:
The data reveals a significant spam issue, especially on certain days. For instance, on January 1st, all viewed posts were spam, which could indicate a day with heavy spam activity or that the system failed to filter out spam content effectively. On January 2nd, the situation improved slightly, but still, half of the viewed posts were spam. This suggests that spam management or filtering needs improvement, potentially through better keyword filtering, machine learning-based spam detection, or user reporting mechanisms. 

The analysis also highlights the importance of continually monitoring and refining spam detection systems to ensure that user engagement isn't dominated by low-quality, spammy content.
