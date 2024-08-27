## Challenge link: https://datalemur.com/questions/alibaba-compressed-mode

# Challenge: Compressed Mode [Alibaba SQL Interview Question]

You're given a table containing the item count for each order on Alibaba, along with the frequency of orders that have the same item count. Write a query to retrieve the mode of the order occurrences. Additionally, if there are multiple item counts with the same mode, the results should be sorted in ascending order.

Clarifications:

* item_count: Represents the number of items sold in each order.
* order_occurrences: Represents the frequency of orders with the corresponding number of items sold per order.
* For example, if there are 800 orders with 3 items sold in each order, the record would have an item_count of 3 and an order_occurrences of 800.

Effective June 14th, 2023, the problem statement has been revised and additional clarification have been added for clarity.

![image](https://github.com/user-attachments/assets/c552cdf7-1f5e-42d7-842d-f9866995559d)


# Answer:

``` sql
SELECT 
  item_count
FROM 
  items_per_order
WHERE
  order_occurrences =(
    SELECT
      MAX(order_occurrences)
    FROM 
      items_per_order);
``` 

### Result:

![image](https://github.com/user-attachments/assets/0eaab6d5-e61f-4a59-ad01-075ccf710ab7)


## Explanation:

You need to find the mode of the `order_occurrences` from the `items_per_order` table. The mode is the value that appears most frequently. If there are multiple values with the same highest frequency, all such values should be returned, sorted in ascending order.

#### Query Explanation

1. **Subquery (`SELECT MAX(order_occurrences) FROM items_per_order`):**
   - This subquery finds the maximum value of `order_occurrences` from the `items_per_order` table. Essentially, it identifies the highest frequency of orders for any given `item_count`.

2. **Main Query:**
   - The `WHERE` clause in the main query uses the result of the subquery to filter the records in the `items_per_order` table.
   - The `SELECT item_count` retrieves the `item_count` values where the `order_occurrences` matches the highest frequency found in the subquery.

### Result Analysis

Given the result:

```
item_count
2
4
```

#### Interpretation

- The result indicates that both `item_count` values `2` and `4` have the same highest frequency of orders. This means that there are two item counts, `2` and `4`, each with the maximum number of occurrences in the table.
- These values are returned in ascending order, which aligns with the requirement that if multiple item counts have the same maximum frequency, they should be sorted.

### Further or Alternative Analysis

#### Alternative Approach

The given query effectively retrieves the desired result, but we can explore a different approach to achieve the same outcome, particularly focusing on achieving a more comprehensive understanding of the data.

1. **Using Common Table Expressions (CTE):**

```sql
WITH FrequencyCTE AS (
  SELECT 
    item_count, 
    order_occurrences
  FROM 
    items_per_order
),
MaxFrequencyCTE AS (
  SELECT
    MAX(order_occurrences) AS max_occurrences
  FROM 
    FrequencyCTE
)
SELECT 
  item_count
FROM 
  FrequencyCTE
WHERE 
  order_occurrences = (SELECT max_occurrences FROM MaxFrequencyCTE)
ORDER BY
  item_count;
```

#### Explanation of the Alternative Query

- **FrequencyCTE:** A CTE to capture the `item_count` and `order_occurrences` from the `items_per_order` table.
- **MaxFrequencyCTE:** A CTE to calculate the maximum `order_occurrences` value.
- **Main Query:** Selects the `item_count` where `order_occurrences` equals the maximum value found in the `MaxFrequencyCTE` and orders the results by `item_count`.

This approach helps in breaking down the problem into manageable parts and can improve readability, especially with complex queries.

#### Conclusion

The original query is succinct and achieves the goal effectively by leveraging subqueries. The alternative approach with CTEs offers a more structured breakdown of the steps involved and can be beneficial for more complex scenarios.

Both methods ensure that the result is correct and meets the requirement of retrieving the mode of the order occurrences and sorting the results if there are multiple values.
