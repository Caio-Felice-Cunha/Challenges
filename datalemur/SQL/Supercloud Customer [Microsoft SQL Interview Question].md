## Challenge link: https://datalemur.com/questions/supercloud-customer

# Challenge: Supercloud Customer [Microsoft SQL Interview Question]

A Microsoft Azure Supercloud customer is defined as a customer who has purchased at least one product from every product category listed in the products table.

Write a query that identifies the customer IDs of these Supercloud customers.

![image](https://github.com/user-attachments/assets/5a648c1d-7b93-4aed-83ff-a775a22d9bd8)

![image](https://github.com/user-attachments/assets/083bc7fe-96cb-4f46-a8f5-e920e4e82a3c)



# Answer:

``` sql
SELECT
  customer_id
FROM(
  SELECT
    customer_contracts.customer_id
    , COUNT(DISTINCT products.product_category) AS Products_Purchased
  FROM
    customer_contracts
  INNER JOIN 
    products
    ON customer_contracts.product_id = products.product_id
  GROUP BY 
    customer_contracts.customer_id
) AS Product_Customer

WHERE
  Products_Purchased = (
    SELECT
      COUNT(DISTINCT product_category)
    FROM
      products
  )

``` 
![image](https://github.com/user-attachments/assets/4aba9705-c869-49b9-80a1-11bc9b1b4906)


## Explanation:

The challenge requires identifying "Supercloud customers," defined as customers who have purchased at least one product from every product category listed in the `products` table. Let's break down the provided SQL query and explain how it accomplishes this task.

#### Step 1: Joining `customer_contracts` and `products` Tables

The query begins by joining the `customer_contracts` table with the `products` table on the `product_id`. This join operation allows us to associate each purchase (recorded in `customer_contracts`) with the corresponding product category from the `products` table.

```sql
FROM
  customer_contracts
INNER JOIN 
  products
  ON customer_contracts.product_id = products.product_id
```

- **`customer_contracts` Table:** Contains records of customers and the products they purchased. It has at least two columns: `customer_id` and `product_id`.
- **`products` Table:** Contains information about the products, including `product_id` and `product_category`.
- **Join Condition:** The `INNER JOIN` ensures that only records with matching `product_id` in both tables are selected.

#### Step 2: Counting the Distinct Product Categories Purchased by Each Customer

Next, the query counts the number of distinct product categories each customer has purchased. This is crucial because we want to know if a customer has purchased from every available product category.

```sql
SELECT
  customer_contracts.customer_id,
  COUNT(DISTINCT products.product_category) AS Products_Purchased
GROUP BY 
  customer_contracts.customer_id
```

- **`COUNT(DISTINCT products.product_category)`:** This counts the number of unique product categories that each customer has purchased.
- **`GROUP BY customer_contracts.customer_id`:** Groups the results by `customer_id`, so we get a count of product categories for each customer.

#### Step 3: Filtering Customers Who Have Purchased from All Product Categories

After counting the distinct product categories purchased by each customer, the query filters out customers who haven't purchased from every product category.

```sql
WHERE
  Products_Purchased = (
    SELECT
      COUNT(DISTINCT product_category)
    FROM
      products
  )
```

- **Subquery:** `(SELECT COUNT(DISTINCT product_category) FROM products)` counts the total number of distinct product categories in the `products` table.
- **Comparison:** The `WHERE` clause checks if the number of product categories a customer has purchased (`Products_Purchased`) is equal to the total number of product categories in the `products` table. If they match, the customer is considered a "Supercloud" customer.

### Output

The final result provides the `customer_id` of customers who meet the criteria, i.e., they have purchased from all product categories.

### Alternative Solution

Another way to approach this problem is by using a `HAVING` clause after performing a join between `customer_contracts` and `products` to filter customers directly.

```sql
SELECT
  customer_contracts.customer_id
FROM
  customer_contracts
INNER JOIN 
  products
  ON customer_contracts.product_id = products.product_id
GROUP BY 
  customer_contracts.customer_id
HAVING 
  COUNT(DISTINCT products.product_category) = (
    SELECT COUNT(DISTINCT product_category) FROM products
  )
```

#### Explanation of the Alternative Solution

- **Join and Grouping:** This solution also joins `customer_contracts` and `products`, then groups by `customer_id`.
- **HAVING Clause:** Instead of using a subquery in the `WHERE` clause, the `HAVING` clause directly filters the grouped results, only retaining customers whose `COUNT(DISTINCT products.product_category)` equals the total number of product categories.

### Data Analysis of the Result

- **Data Consistency:** Both solutions guarantee that the customer IDs returned are those who have purchased from all categories. The output will be consistent as long as the `products` and `customer_contracts` tables are properly maintained.
- **Performance Considerations:** 
  - If the tables are large, the subquery inside the `WHERE` clause might add some overhead. The alternative solution with the `HAVING` clause might be slightly more efficient because it avoids the additional filtering step.
  - Indexing on `product_id` and `product_category` would improve performance, especially on large datasets.

### Comment

The provided solution is effective and accurate for identifying Supercloud customers. The alternative solution achieves the same result but might be more straightforward in terms of readability. Both approaches rely on accurate data in the `customer_contracts` and `products` tables, and they assume that every customer is represented in `customer_contracts`. This logic is particularly useful in scenarios involving customer segmentation, product penetration analysis, and cross-category purchase behaviors.
