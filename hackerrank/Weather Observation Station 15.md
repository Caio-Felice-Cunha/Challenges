## Challenge link: https://www.hackerrank.com/challenges/weather-observation-station-15/problem?isFullScreen=true

# Challenge: hackerrank - Weather Observation Station 15
Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to 4 decimal places.

Input Format

The STATION table is described as follows:
![image](https://github.com/user-attachments/assets/cecca00a-806e-45d3-9314-40d92c18a13a)


# Answer:

``` sql
SELECT
    ROUND(LONG_W, 4)
FROM
    STATION
WHERE
    LAT_N=
        (SELECT
            MAX(LAT_N)
        FROM
            STATION
        WHERE
            LAT_N < 137.2345)
            
```
![image](https://github.com/user-attachments/assets/338de770-f180-416c-ae6c-137fb788cfe2)
