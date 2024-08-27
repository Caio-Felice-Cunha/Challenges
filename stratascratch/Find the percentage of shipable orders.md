## Challenge link: https://platform.stratascratch.com/coding/10090-find-the-percentage-of-shipable-orders?code_type=3

# Challenge: Find the percentage of shipable orders

![image](https://github.com/user-attachments/assets/daad26e7-5f7c-4ff5-ae8f-faa8ffcb7280)

Find the percentage of shipable orders.
Consider an order is shipable if the customer's address is known.

![image](https://github.com/user-attachments/assets/2b6acc96-9cb0-4da1-8de6-70cbe20e1e62)


# Answer:

``` sql
SELECT 
    100 *
    COUNT(CASE 
        WHEN ADDRESS IS NOT NULL THEN 'Shipable'
        ELSE NULL
    END)
    / COUNT(*) AS Perc_Shipable
FROM 
    orders
JOIN 
    customers
    ON orders.cust_id  = customers.id

```

### Result:
![image](https://github.com/user-attachments/assets/48fca7b7-85b8-44cc-94ff-3392b6c88f58)

## Explanation:

The SQL code solves the challenge of finding the percentage of shippable orders, where an order is considered shippable if the customer's address is known. Let's break down the code step by step:

#### 1. `SELECT` Clause
The `SELECT` clause is used to specify the columns or calculations that we want to retrieve from the database. In this case, we are calculating the percentage of shippable orders.

#### 2. `100 * COUNT(CASE WHEN ADDRESS IS NOT NULL THEN 'Shipable' ELSE NULL END)`
- **`CASE WHEN ADDRESS IS NOT NULL THEN 'Shipable' ELSE NULL END`**:
  - This part checks if the `ADDRESS` field in the `customers` table is **not null** (i.e., the customer's address is known).
  - If the condition is true (the address is known), it returns `'Shipable'`.
  - If the condition is false (the address is unknown), it returns `NULL`.

- **`COUNT(...)`**:
  - The `COUNT` function counts the number of non-null values returned by the `CASE` statement.
  - This effectively counts the number of orders that are shippable (i.e., orders associated with customers who have a known address).

- **`100 * ... / COUNT(*)`**:
  - This multiplies the count of shippable orders by 100 to calculate the percentage.
  - The result is then divided by the total number of orders (`COUNT(*)`), giving the percentage of shippable orders.

#### 3. `FROM orders JOIN customers ON orders.cust_id = customers.id`
- **`FROM orders`**:
  - This specifies the primary table, `orders`, from which we are retrieving data.

- **`JOIN customers ON orders.cust_id = customers.id`**:
  - The `JOIN` clause combines rows from the `orders` table with rows from the `customers` table based on a related column.
  - In this case, the join is performed on the `cust_id` column in the `orders` table and the `id` column in the `customers` table. This ensures that each order is associated with the correct customer.

#### 4. `AS Perc_Shipable`
- This alias renames the resulting calculation as `Perc_Shipable`, making it clear that the value represents the percentage of shippable orders.

### Data Analysis of the Result

The result of this query is a single value that represents the percentage of orders that are shippable. In this specific case, the result is `28`, which means that 28% of the total orders are shippable. 

#### Interpretation:
- **Perc_Shipable = 28**: Out of all the orders in the database, only 28% have a customer with a known address. This indicates that 72% of the orders have customers with unknown addresses, meaning they cannot be shipped.

### Comment on the Result
A percentage of 28% for shippable orders is relatively low, which could be a significant issue for the business. This low percentage suggests that a large portion of the customer data is incomplete or incorrect, specifically in terms of missing addresses. This could lead to operational inefficiencies, such as delays in shipping or even the inability to fulfill a significant portion of orders.

### Further or Alternative Analysis

To gain a deeper understanding or explore alternative perspectives, consider the following analyses:

1. **Breakdown by Region or Customer Type**:
   - Perform a more granular analysis by breaking down the shippable percentage by regions or customer types. This could help identify specific areas or segments where the address information is more or less complete.
   - Example query:
     ```sql
     SELECT 
         region,
         100 * COUNT(CASE WHEN ADDRESS IS NOT NULL THEN 'Shipable' ELSE NULL END) / COUNT(*) AS Perc_Shipable
     FROM 
         orders
     JOIN 
         customers
         ON orders.cust_id = customers.id
     GROUP BY 
         region;
     ```

2. **Identify Root Cause**:
   - Investigate the root cause of the missing address information. This could involve checking the process of customer data collection, validation, and storage. 
   - Example query to identify orders with missing addresses:
     ```sql
     SELECT 
         orders.order_id, 
         customers.id, 
         customers.name, 
         customers.contact_info
     FROM 
         orders
     JOIN 
         customers 
         ON orders.cust_id = customers.id
     WHERE 
         customers.address IS NULL;
     ```
   - This query will return a list of orders where the customer’s address is missing, allowing the team to address these specific cases.

3. **Analyze the Impact on Revenue**:
   - Assess how the low percentage of shippable orders might impact revenue. Calculate the potential revenue loss due to unshippable orders and explore ways to mitigate this issue.

4. **Improvement in Data Collection**:
   - Propose improvements in data collection processes to ensure that customer addresses are captured more reliably. This might involve adding mandatory address fields in customer registration forms or implementing better validation checks.

### Conclusion

The SQL query provided offers a straightforward solution to calculate the percentage of shippable orders. However, the low percentage of 28% indicates a significant issue with missing address data. Further analysis is recommended to understand the root cause, segment the data for more detailed insights, and assess the business impact. Implementing improved data collection practices could help increase the percentage of shippable orders, thereby improving operational efficiency and customer satisfaction.
