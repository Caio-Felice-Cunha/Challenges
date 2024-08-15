## Challenge link: https://www.hackerrank.com/challenges/african-cities/problem?isFullScreen=true

# Challenge: African Cities

Given the CITY and COUNTRY tables, query the names of all cities where the CONTINENT is 'Africa'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

Input Format

The CITY and COUNTRY tables are described as follows:

![image](https://github.com/user-attachments/assets/33f387cd-e534-475d-a24c-d198cdf7c148)

![image](https://github.com/user-attachments/assets/b5ecc67a-c3f3-49b7-ad3d-f885eb124ebf)

# Answer:

``` sql
SELECT
    CITY.NAME
FROM
    CITY
JOIN
    COUNTRY
ON
    CITY.CountryCode = COUNTRY.Code
WHERE
    COUNTRY.CONTINENT = 'Africa'
``` 
![image](https://github.com/user-attachments/assets/5c8aa72d-aaae-4b30-8328-53e9b139efaf)

## Explanation:
To solve the problem of querying the names of all cities in Africa from the `CITY` and `COUNTRY` tables, we can break down the provided solution into several key steps. Here's a detailed explanation of the SQL code and a discussion of possible alternative solutions:

### **Tables Overview:**
1. **CITY Table**:
    - `Name`: The name of the city.
    - `CountryCode`: The country code associated with the city.

2. **COUNTRY Table**:
    - `Code`: The unique code for each country, which corresponds to `CountryCode` in the `CITY` table.
    - `Continent`: The continent to which the country belongs (e.g., Africa, Asia, etc.).

### **Solution Breakdown:**

```sql
SELECT
    CITY.NAME
FROM
    CITY
JOIN
    COUNTRY
ON
    CITY.CountryCode = COUNTRY.Code
WHERE
    COUNTRY.CONTINENT = 'Africa'
```

### **Step-by-Step Breakdown:**

1. **SELECT Clause:**
   - The `SELECT` statement specifies which columns you want to retrieve. In this case, you want to retrieve the `NAME` column from the `CITY` table, which represents the names of the cities.
   
   ```sql
   SELECT CITY.NAME
   ```

2. **FROM Clause:**
   - The `FROM` clause indicates the primary table from which to retrieve data. Here, we start with the `CITY` table.
   
   ```sql
   FROM CITY
   ```

3. **JOIN Clause:**
   - A `JOIN` operation is used to combine rows from two or more tables based on a related column. Here, the `CITY` table is joined with the `COUNTRY` table on the condition that `CITY.CountryCode` matches `COUNTRY.Code`.
   - This is known as an inner join, which means only records with matching values in both tables will be included in the result.

   ```sql
   JOIN COUNTRY
   ON CITY.CountryCode = COUNTRY.Code
   ```

4. **WHERE Clause:**
   - The `WHERE` clause filters the results to include only those records that meet a specified condition. In this case, it filters the data to only include cities where the `COUNTRY.CONTINENT` is 'Africa'.
   
   ```sql
   WHERE COUNTRY.CONTINENT = 'Africa'
   ```

### **Alternative Solution:**
The given SQL query is an optimal way to retrieve the required information. However, another way to achieve the same result is by using a **Subquery**:

```sql
SELECT NAME
FROM CITY
WHERE CountryCode IN (
    SELECT Code
    FROM COUNTRY
    WHERE Continent = 'Africa'
)
```

### **Explanation of the Alternative Solution:**

1. **Subquery (Inner Query):**
   - The subquery (`SELECT Code FROM COUNTRY WHERE Continent = 'Africa'`) retrieves the `Code` of all countries located in Africa.
   
   ```sql
   SELECT Code
   FROM COUNTRY
   WHERE Continent = 'Africa'
   ```

2. **Main Query:**
   - The main query (`SELECT NAME FROM CITY WHERE CountryCode IN (...)`) selects the `NAME` of cities where the `CountryCode` is in the list of codes retrieved by the subquery.
   
   ```sql
   SELECT NAME
   FROM CITY
   WHERE CountryCode IN (...)
   ```

### **Comparison of the Two Solutions:**

- **Performance**: 
  - The initial `JOIN` solution is generally more efficient because it directly relates the tables using an `INNER JOIN`, which can be optimized by the database engine.
  - The subquery solution can be less efficient if the subquery retrieves a large number of rows, as the main query has to filter through all of them.
  
- **Readability**:
  - The `JOIN` solution is often more readable and preferred in SQL, especially when working with relational data where tables are linked by foreign keys.
  - The subquery approach can be more intuitive for people who prefer to think of filtering data step-by-step.

### **Conclusion:**
The `JOIN` method provided in the initial solution is the most efficient and straightforward way to solve the problem. The alternative solution using a subquery is also valid and might be useful in more complex scenarios where multiple layers of filtering are needed. However, for this particular case, the `JOIN` approach is preferable.
