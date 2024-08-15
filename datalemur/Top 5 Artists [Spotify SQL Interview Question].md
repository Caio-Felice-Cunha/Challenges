## Challenge link: https://datalemur.com/questions/top-fans-rank

# Challenge: Top 5 Artists [Spotify SQL Interview Question]

Assume there are three Spotify tables: artists, songs, and global_song_rank, which contain information about the artists, songs, and music charts, respectively.

Write a query to find the top 5 artists whose songs appear most frequently in the Top 10 of the global_song_rank table. Display the top 5 artist names in ascending order, along with their song appearance ranking.

If two or more artists have the same number of song appearances, they should be assigned the same ranking, and the rank numbers should be continuous (i.e. 1, 2, 2, 3, 4, 5). If you've never seen a rank order like this before, do the rank window function tutorial.

![image](https://github.com/user-attachments/assets/b742bfec-1884-432b-a5a1-642a18459e50) ![image](https://github.com/user-attachments/assets/889ebd3d-fd20-4ed6-9197-9240a7701183) ![image](https://github.com/user-attachments/assets/a48f8f31-6f8c-4b36-a429-fbdd7aa44ad1)

# Answer:

``` sql
SELECT
  Artist_Name
  , Artists_rank
FROM
  (SELECT 
    artists.artist_name AS Artist_Name
    , DENSE_RANK()
      OVER(
        ORDER BY COUNT(songs.song_id) DESC) AS Artists_rank
  FROM 
    artists
  JOIN 
    songs
  ON 
    artists.artist_id = songs.artist_id
  JOIN 
    global_song_rank
  ON  
    songs.song_id = global_song_rank.song_id
  WHERE 
    global_song_rank.rank <=10
  GROUP BY
    artists.artist_name) AS Artists_Rank_Table
WHERE
  Artists_rank <= 5

```
![image](https://github.com/user-attachments/assets/5a58d3ce-ed30-4b50-a40d-5080eae5f071)


## Explanation:
### Breakdown of the Solution

The goal of this SQL query is to find the top 5 artists whose songs appear most frequently in the Top 10 of the `global_song_rank` table and display their names along with their rankings.

#### Step-by-Step Explanation:

1. **Identify the Required Tables and Columns:**
   - **`artists`:** Contains information about artists.
   - **`songs`:** Contains information about songs, including a reference to the artist via `artist_id`.
   - **`global_song_rank`:** Contains the global ranking of songs, where the `rank` column indicates a song's position on the chart.

2. **Perform the Joins:**
   ```sql
   FROM 
     artists
   JOIN 
     songs
   ON 
     artists.artist_id = songs.artist_id
   JOIN 
     global_song_rank
   ON  
     songs.song_id = global_song_rank.song_id
   ```
   - **First Join:** The `artists` table is joined with the `songs` table using the `artist_id` column. This allows us to associate each song with its respective artist.
   - **Second Join:** The result is then joined with the `global_song_rank` table using the `song_id` column. This links each song with its global ranking.

3. **Filter for Top 10 Songs:**
   ```sql
   WHERE 
     global_song_rank.rank <= 10
   ```
   - This filter ensures that only songs that have a rank of 10 or better (Top 10) are considered in the final result.

4. **Aggregate the Data:**
   ```sql
   GROUP BY
     artists.artist_name
   ```
   - The `GROUP BY` clause groups the results by the artist's name. This allows us to count the number of Top 10 songs each artist has.

5. **Rank the Artists:**
   ```sql
   , DENSE_RANK()
     OVER(
       ORDER BY COUNT(songs.song_id) DESC) AS Artists_rank
   ```
   - **`DENSE_RANK()` Window Function:** This function ranks the artists based on the number of Top 10 songs they have, with the `ORDER BY COUNT(songs.song_id) DESC` clause ensuring that the artist with the most Top 10 songs is ranked first.
   - The `DENSE_RANK()` function ensures that if two artists have the same number of Top 10 songs, they receive the same rank, and the subsequent rank is not skipped.

6. **Filter for the Top 5 Artists:**
   ```sql
   WHERE
     Artists_rank <= 5
   ```
   - This filter limits the results to only the top 5 ranked artists.

7. **Final Selection:**
   ```sql
   SELECT
     Artist_Name
     , Artists_rank
   ```
   - Finally, the query selects and displays the artist names and their respective ranks.

### Alternative Solution

An alternative approach to solving this problem could involve using a Common Table Expression (CTE) to organize the query more clearly:

```sql
WITH Top10Songs AS (
  SELECT 
    artists.artist_name AS Artist_Name,
    COUNT(*) AS Song_Appearances
  FROM 
    artists
  JOIN 
    songs ON artists.artist_id = songs.artist_id
  JOIN 
    global_song_rank ON songs.song_id = global_song_rank.song_id
  WHERE 
    global_song_rank.rank <= 10
  GROUP BY 
    artists.artist_name
),
RankedArtists AS (
  SELECT 
    Artist_Name,
    DENSE_RANK() OVER (ORDER BY Song_Appearances DESC) AS Artists_rank
  FROM 
    Top10Songs
)
SELECT 
  Artist_Name,
  Artists_rank
FROM 
  RankedArtists
WHERE 
  Artists_rank <= 5;
```

#### Explanation of the Alternative Solution:

1. **CTE `Top10Songs`:** 
   - This CTE performs the initial join and filtering, aggregating the number of Top 10 songs each artist has.

2. **CTE `RankedArtists`:**
   - This CTE applies the `DENSE_RANK()` function to the results of `Top10Songs`, ranking the artists based on the number of Top 10 appearances.

3. **Final Query:**
   - The main query selects the top 5 artists from `RankedArtists`, similar to the original solution.

#### Advantages of the Alternative Solution:
- **Readability:** The use of CTEs makes the query easier to read and maintain, especially for more complex queries.
- **Reusability:** If you need to extend the query further (e.g., adding more filters or computations), CTEs provide a more structured way to build upon the base results.

Both the original and alternative solutions effectively achieve the desired outcome, with the main difference being in how the query is structured. The use of CTEs in the alternative solution can improve readability and organization in more complex scenarios.
