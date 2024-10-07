## Challenge link: https://www.hackerrank.com/challenges/weather-observation-station-20/problem?isFullScreen=true

# Challenge: Weather Observation Station 20

A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to 4 decimal places.

Input Format

The STATION table is described as follows:

![image](https://github.com/user-attachments/assets/2e30b220-d5ca-48a9-8451-5b5d393abfaf)


# Answer:

``` sql
WITH Ranking AS(
    SELECT 
        LAT_N
        , ROW_NUMBER() 
            OVER (ORDER BY lat_n) AS A
        , COUNT(*) 
            OVER () AS Count_totals
    FROM 
        STATION
), 
Medians AS (
    SELECT 
        LAT_N
    FROM
        Ranking
    WHERE
        A = FLOOR((Count_totals + 1) / 2)
            OR A = CEIL((Count_totals + 1) / 2)
)

SELECT 
    ROUND(
        AVG(LAT_N), 
    4) AS Median
FROM 
    Medians;
```
![image](https://github.com/user-attachments/assets/a9c1d490-c579-44f8-9554-453a03a4f75d)
