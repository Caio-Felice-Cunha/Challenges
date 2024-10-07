## Challenge link: https://www.hackerrank.com/challenges/weather-observation-station-19/problem?isFullScreen=true

# Challenge:Weather Observation Station 19

Consider P1(a,b) and P2(c,d) to be two points on a 2D plane where (a,b) are the respective minimum and maximum values of Northern Latitude (LAT_N) and (c,d) are the respective minimum and maximum values of Western Longitude (LONG_W) in STATION.

Query the Euclidean Distance between points P1 and P2 and format your answer to display 4 decimal digits.

Input Format

The STATION table is described as follows:

![image](https://github.com/user-attachments/assets/7fba6379-1f8b-4caf-a1ac-b54d538a0f3e)


# Answer:

``` sql
SELECT 
    ROUND
        (SQRT
            (POWER((
                MIN(LAT_N)-MAX(LAT_N)),2) 
            + 
            POWER((
                MIN(LONG_W)-MAX(LONG_W)),2))
            ,4) AS EUCLIDEAN_DISTANCE
FROM 
    STATION;
```

![image](https://github.com/user-attachments/assets/9a080ac1-6f45-404d-9bd8-033da9173e39)

