## Challenge link: https://www.hackerrank.com/challenges/asian-population/problem?isFullScreen=true

# Challenge: Population Census

Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

Input Format

The CITY and COUNTRY tables are described as follows:

![image](https://github.com/user-attachments/assets/16125fea-88e3-4b0a-b3fe-aa26703b18d4)

![image](https://github.com/user-attachments/assets/0fa78c60-37aa-4a5a-b631-f75569b1e81c)


# Answer:

``` sql
SELECT
  SUM(CITY.POPULATION) AS Total_Population
 FROM
    CITY
JOIN
    COUNTRY
ON
    CITY.CountryCode = COUNTRY.Code
WHERE 
    COUNTRY.CONTINENT = 'Asia'
```

![image](https://github.com/user-attachments/assets/b9f98778-208c-4b85-95ea-13568480b85c)

## Explanation:
The challenge requires querying the total population of all cities in Asia using two tables: `CITY` and `COUNTRY`. Here's how the provided SQL query solves the problem:

1. **`SELECT SUM(CITY.POPULATION) AS Total_Population`:**
   - This line selects the sum of the `POPULATION` column from the `CITY` table. The `SUM` function aggregates the populations of all cities that match the criteria specified in the query. The result is labeled as `Total_Population`.

2. **`FROM CITY`:**
   - This specifies that the query will pull data from the `CITY` table.

3. **`JOIN COUNTRY ON CITY.CountryCode = COUNTRY.Code`:**
   - The `JOIN` operation is used to combine rows from the `CITY` and `COUNTRY` tables based on a related column. 
   - Here, `CITY.CountryCode` is matched with `COUNTRY.Code` to link each city with its corresponding country. This allows the query to access information from both tables in a single result set.

4. **`WHERE COUNTRY.CONTINENT = 'Asia'`:**
   - The `WHERE` clause filters the combined data so that only cities located in countries on the continent of 'Asia' are considered.
   - This filtering is done by checking the `CONTINENT` column in the `COUNTRY` table and including only those rows where the continent is 'Asia'.

### Summary:
The query effectively calculates the total population of cities located in countries on the continent of Asia by:
- Linking cities to their respective countries using the `CountryCode` and `Code` columns.
- Filtering for countries on the continent of Asia.
- Summing the populations of these filtered cities to get the `Total_Population`.
