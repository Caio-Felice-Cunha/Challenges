## Challenge link: https://datalemur.com/questions/median-search-freq

# Challenge: Median Google Search Frequency (I need help to finish this)

Google's marketing team is making a Superbowl commercial and needs a simple statistic to put on their TV ad: the median number of searches a person made last year.

However, at Google scale, querying the 2 trillion searches is too costly. Luckily, you have access to the summary table which tells you the number of searches made last year and how many Google users fall into that bucket.

Write a query to report the median of searches made by a user. Round the median to one decimal point.


<img width="381" height="579" alt="Screenshot 2026-01-22 075115" src="https://github.com/user-attachments/assets/f20ba24e-9a88-430f-9237-ece3e576540f" />


# Answer:

``` sql
-- HELP ME!!!!!

WITH ordered AS (
  SELECT
    searches,
    num_users,
    SUM(num_users) OVER (ORDER BY searches) AS cum_up,
    SUM(num_users) OVER () AS total_users
  FROM search_frequency
),

ranges AS (
  SELECT
    searches,
    cum_up - num_users + 1 AS cum_lo,
    cum_up AS cum_hi,
    total_users
  FROM ordered
)

SELECT
  ROUND(AVG(val), 1) AS median
FROM (
  SELECT
    searches AS val
  FROM ranges
  WHERE
    -- if total is odd, both positions collapse to the same
    
    --WHAT SHOULD I DO???? HELP!!!!
) AS median_positions;

``` 

### Result:

## Explanation:
