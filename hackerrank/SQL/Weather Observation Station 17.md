## Challenge link: https://www.hackerrank.com/challenges/weather-observation-station-17/problem?isFullScreen=true

# Challenge: Weather Observation Station 17

Query the Western Longitude (LONG_W)where the smallest Northern Latitude (LAT_N) in STATION is greater than 38.7780. Round your answer to 4 decimal places.

Input Format

The STATION table is described as follows:

![image](https://github.com/user-attachments/assets/83b19da5-ecf5-4212-a555-7478b37e3048)


# Answer:

``` sql
SELECT  
    ROUND(LONG_W,4) 
FROM 
    STATION 
WHERE 
    LAT_N > 38.7780
ORDER BY 
    LAT_N ASC
LIMIT 1
```

![image](https://github.com/user-attachments/assets/fe377fea-056d-4eaa-b69e-cc8974a9bc69)
