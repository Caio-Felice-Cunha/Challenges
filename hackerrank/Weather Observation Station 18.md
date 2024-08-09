## Challenge link: https://www.hackerrank.com/challenges/weather-observation-station-18/problem?isFullScreen=true

# Challenge: Weather Observation Station 18

Consider P1(a,b) and P2(c,d) to be two points on a 2D plane.

 happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
 happens to equal the minimum value in Western Longitude (LONG_W in STATION).
 happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
 happens to equal the maximum value in Western Longitude (LONG_W in STATION).
Query the Manhattan Distance between points P1 and P2 and round it to a scale of 4 decimal places.

Input Format

The STATION table is described as follows:

![image](https://github.com/user-attachments/assets/00270440-3867-4402-9647-f1a4b9d743cd)


# Answer:

``` sql
SELECT 
    ROUND(
        ABS(
            MAX(LAT_N) - MIN(LAT_N)) 
        +
        ABS(
            MAX(LONG_W) - MIN(LONG_W)), 
        4) 
FROM 
    STATION;
```

![image](https://github.com/user-attachments/assets/1becf27a-c821-43d6-9ffd-b8b8fb9eb3ca)
