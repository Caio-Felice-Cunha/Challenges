## Challenge link: https://www.hackerrank.com/challenges/harry-potter-and-wands/problem?isFullScreen=true

# Challenge: Ollivander's Inventory

Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.

Input Format

The following tables contain data on the wands in Ollivander's inventory:

Wands: The id is the id of the wand, code is the code of the wand, coins_needed is the total number of gold galleons needed to buy the wand, and power denotes the quality of the wand (the higher the power, the better the wand is).

![image](https://github.com/user-attachments/assets/51dfb0a2-3660-4121-891c-7333eae028f8)

Wands_Property: The code is the code of the wand, age is the age of the wand, and is_evil denotes whether the wand is good for the dark arts. If the value of is_evil is 0, it means that the wand is not evil. The mapping between code and age is one-one, meaning that if there are two pairs,
![image](https://github.com/user-attachments/assets/74b54838-f3c4-407f-ae27-a9ddcb8501c5)


![image](https://github.com/user-attachments/assets/ad3164c0-f48a-40da-8ac9-6d746d8a10f5)


# Answer:

``` sql
SELECT 
    wands.id
    , Wands_Property.age
    ,wands.coins_needed
    ,wands.power 
FROM 
    wands
JOIN 
    Wands_Property 
    ON wands.code = Wands_Property.code 
WHERE 
    Wands_Property.is_evil=0 
    AND wands.coins_needed = 
        (SELECT 
            MIN(WANDS1.coins_needed) 
        FROM 
         wands WANDS1 
        JOIN 
            Wands_Property Wands_Property1 
            ON WANDS1.code = Wands_Property1.code 
         WHERE wands.power = WANDS1.power 
                        AND Wands_Property.age = Wands_Property1.age
    ) 
ORDER BY 
    wands.POWER DESC
    , Wands_Property.age DESC
;
```

![image](https://github.com/user-attachments/assets/8b5eae04-5f27-40ec-986b-518942d953e9)


## Explanation:

### **Solution Breakdown**

Let's break down the SQL query step by step to understand how it solves the problem:

1. **Selecting the Columns:**
   ```sql
   SELECT 
       wands.id,
       Wands_Property.age,
       wands.coins_needed,
       wands.power 
   ```
   - This line specifies the columns we want to retrieve in the result set:
     - `wands.id`: The unique identifier of the wand.
     - `Wands_Property.age`: The age of the wand, retrieved from the `Wands_Property` table.
     - `wands.coins_needed`: The cost of the wand in gold galleons.
     - `wands.power`: The power level of the wand.

2. **Joining Tables:**
   ```sql
   FROM 
       wands
   JOIN 
       Wands_Property 
       ON wands.code = Wands_Property.code 
   ```
   - The `JOIN` operation combines data from the `wands` and `Wands_Property` tables. The tables are linked via the `code` column, which is common to both tables.
   - This allows us to retrieve additional properties of the wands (like age and whether they are evil) alongside the wand information.

3. **Filtering Non-Evil Wands:**
   ```sql
   WHERE 
       Wands_Property.is_evil = 0 
   ```
   - This condition filters out all wands that are marked as "evil" (`is_evil = 1`). Only non-evil wands (`is_evil = 0`) are considered.

4. **Finding Minimum Coins Needed:**
   ```sql
   AND wands.coins_needed = 
       (SELECT 
           MIN(WANDS1.coins_needed) 
        FROM 
           wands WANDS1 
        JOIN 
           Wands_Property Wands_Property1 
           ON WANDS1.code = Wands_Property1.code 
        WHERE wands.power = WANDS1.power 
              AND Wands_Property.age = Wands_Property1.age
   ) 
   ```
   - This subquery finds the minimum number of coins needed to purchase a wand with the same power and age.
   - For each wand in the outer query, this condition ensures that we only select wands that have the lowest possible price (`MIN(WANDS1.coins_needed)`) for their specific combination of power and age.

5. **Sorting the Results:**
   ```sql
   ORDER BY 
       wands.power DESC,
       Wands_Property.age DESC
   ```
   - The results are sorted primarily by `wands.power` in descending order, ensuring that wands with higher power appear first.
   - If two wands have the same power, they are further sorted by `Wands_Property.age` in descending order, with older wands appearing first.

### **Data Analysis of the Result**

Based on the output snippet provided:

- **Power Level**: All wands in the provided result have a power level of 10, the maximum observed in the data.
- **Age**: The wands vary in age, with older wands appearing first due to the sorting order. For example, the oldest wand with a power of 10 is 496 years old, while the youngest is 395 years old.
- **Coins Needed**: The cost of wands varies significantly. Even though all wands listed have the same power (10), the price difference is vast, ranging from as low as 997 coins to as high as 9831 coins.

#### **Comment:**
The query successfully filters out non-evil wands, ensuring that only the most cost-effective options are listed for each combination of power and age. The sorting by power and age is effective in highlighting the most powerful and oldest wands first. However, even among wands with the same power, the price disparity suggests that other factors (like the wand's history or material) might influence the cost, which isn't directly addressed in the query.

### **Further or Alternative Analysis**

#### **Alternative Approach:**
One could conduct a further analysis by considering additional attributes of the wands that might influence their desirability or cost. For instance:

1. **Material or Core Analysis**: If the `Wands_Property` table contains data on the wand's material (wood type, core type), this could be factored into the analysis. For example, rare materials might explain higher costs even for wands of the same power and age.

2. **Historical Significance**: If historical significance or prior ownership influences the price, adding a filter or sort condition based on these attributes might yield different insights.

3. **Group by Power and Age**:
   - You could aggregate data to find the average or median cost of wands by power and age to get a sense of typical pricing patterns. 
   - This could help identify outliers (wands that are unusually expensive or cheap for their power level and age).

   Example query:
   ```sql
   SELECT 
       wands.power,
       Wands_Property.age,
       AVG(wands.coins_needed) AS avg_cost,
       MEDIAN(wands.coins_needed) AS median_cost
   FROM 
       wands
   JOIN 
       Wands_Property 
       ON wands.code = Wands_Property.code 
   WHERE 
       Wands_Property.is_evil = 0 
   GROUP BY 
       wands.power, 
       Wands_Property.age
   ORDER BY 
       wands.power DESC,
       Wands_Property.age DESC;
   ```
   - This would give an overview of the typical cost distribution and help identify if any wands are overpriced or underpriced relative to their peers.

This further analysis could provide deeper insights into the factors driving wand prices, beyond just power and age.
