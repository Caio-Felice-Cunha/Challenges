## Challenge link: https://www.hackerrank.com/challenges/weather-observation-station-16/problem?isFullScreen=true

# Challenge: Weather Observation Station 16

Query the smallest Northern Latitude (LAT_N) from STATION that is greater than 38.7780. Round your answer to 4 decimal places.

Input Format

The STATION table is described as follows:

![image](https://github.com/user-attachments/assets/f9dff837-5518-4650-884e-1146fbe6968a)


# Answer:

``` sql
SELECT
   ROUND(MIN( LAT_N),4)
FROM
    STATION
WHERE
    LAT_N > 38.7780
```

![image](https://github.com/user-attachments/assets/b7692761-ca3d-45af-8755-954524c0f339)
