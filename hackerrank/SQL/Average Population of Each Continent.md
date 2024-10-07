## Challenge link: https://www.hackerrank.com/challenges/average-population-of-each-continent/problem?isFullScreen=true

# Challenge: Average Population of Each Continent

Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

Input Format

The CITY and COUNTRY tables are described as follows:

![image](https://github.com/user-attachments/assets/a87d7e99-857f-479b-85f8-a66cd54c6784)

![image](https://github.com/user-attachments/assets/cb52ac85-33bd-43cd-a51c-5d2ee8f89a17)

# Answer:

``` sql
SELECT
    COUNTRY.Continent
    , FLOOR(AVG(CITY.Population)) AS AVG_POP
FROM
    CITY
JOIN
    COUNTRY
ON
    CITY.CountryCode = COUNTRY.Code
GROUP BY
    COUNTRY.Continent;
``` 
![image](https://github.com/user-attachments/assets/1ac4fe03-7cdf-44ea-ad72-bc4676acace9)

## Explanation:

To solve the challenge of finding the average population of cities on each continent, you need to perform an SQL query that joins two tables: `CITY` and `COUNTRY`. Here's a breakdown of the provided solution, as well as an alternative approach that could also work:

### Understanding the Tables

1. **CITY Table**:
   - Contains data about cities, including:
     - `Population`: The population of the city.
     - `CountryCode`: A code that links the city to the country it is located in.

2. **COUNTRY Table**:
   - Contains data about countries, including:
     - `Code`: A unique identifier for each country.
     - `Continent`: The continent where the country is located.

### Challenge Requirements

- **Objective**: Find the average population of cities for each continent.
- **Note**: The average should be rounded down to the nearest integer.
- **Key Point**: The `CITY.CountryCode` is a foreign key that matches the `COUNTRY.Code`.

### Provided Solution Breakdown

```sql
SELECT
    COUNTRY.Continent,
    FLOOR(AVG(CITY.Population)) AS AVG_POP
FROM
    CITY
JOIN
    COUNTRY
ON
    CITY.CountryCode = COUNTRY.Code
GROUP BY
    COUNTRY.Continent;
```

#### Explanation:

1. **SELECT Clause**:
   - `COUNTRY.Continent`: This selects the continent name.
   - `FLOOR(AVG(CITY.Population)) AS AVG_POP`: 
     - `AVG(CITY.Population)`: This calculates the average population of the cities within each continent.
     - `FLOOR(...)`: This function rounds down the average population to the nearest integer.

2. **FROM Clause**:
   - Specifies the tables involved in the query (`CITY` and `COUNTRY`).

3. **JOIN Clause**:
   - `JOIN COUNTRY ON CITY.CountryCode = COUNTRY.Code`: 
     - This joins the `CITY` table with the `COUNTRY` table where the `CountryCode` in the `CITY` table matches the `Code` in the `COUNTRY` table.

4. **GROUP BY Clause**:
   - `GROUP BY COUNTRY.Continent`: 
     - This groups the results by continent, so the average population is calculated separately for each continent.

### Step-by-Step Execution:

1. **Join the Tables**: The query first joins the `CITY` table with the `COUNTRY` table using the `CountryCode` from the `CITY` table and the `Code` from the `COUNTRY` table. This allows the query to match each city with the corresponding country and continent.
   
2. **Group by Continent**: After joining, the results are grouped by `COUNTRY.Continent`, meaning that all cities belonging to the same continent are considered together.

3. **Calculate the Average Population**: For each continent, the query calculates the average population of the cities. 

4. **Round Down the Average**: The `FLOOR` function is used to round down the average to the nearest integer.

5. **Display the Results**: Finally, the query outputs the continent name and the rounded-down average population of its cities.

### Alternative Solution

An alternative approach would be to use a **Subquery** to calculate the population first and then apply the rounding function:

```sql
SELECT
    Continent,
    AVG_POP
FROM
(
    SELECT
        COUNTRY.Continent,
        FLOOR(AVG(CITY.Population)) AS AVG_POP
    FROM
        CITY
    JOIN
        COUNTRY
    ON
        CITY.CountryCode = COUNTRY.Code
    GROUP BY
        COUNTRY.Continent
) AS Subquery;
```

#### Explanation of the Alternative Solution:

- **Subquery**: 
  - The subquery performs the same operations as the main query in the original solution (i.e., joining, grouping, and averaging).
  - It creates a temporary table-like result set with the continents and their rounded average populations.

- **Main Query**: 
  - The main query simply selects data from the subquery. This approach is more modular and can be useful if further operations or transformations were needed after calculating the averages.

### Comparing the Solutions

- **Efficiency**: The first solution is more straightforward and efficient for this specific task, as it performs all operations in a single query.
- **Modularity**: The alternative solution might be more appropriate if additional processing is required on the resulting dataset before displaying the final output.

### Conclusion

The provided solution is efficient and clear, using SQL functions to achieve the desired results in one step. The alternative solution introduces a subquery to achieve the same results, which could be useful for more complex scenarios. Both approaches are valid, with the first being the most optimal for the task at hand.
