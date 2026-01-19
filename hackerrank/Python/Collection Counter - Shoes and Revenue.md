## Challenge link: https://www.hackerrank.com/challenges/collections-counter/problem?isFullScreen=true

# Challenge:
A counter is a container that stores elements as dictionary keys, and their counts are stored as dictionary values.

Sample Code
``` text
>>> from collections import Counter
>>> 
>>> myList = [1,1,2,3,4,5,3,2,3,4,2,1,2,3]
>>> print Counter(myList)
Counter({2: 4, 3: 4, 1: 3, 4: 2, 5: 1})
>>>
>>> print Counter(myList).items()
[(1, 3), (2, 4), (3, 4), (4, 2), (5, 1)]
>>> 
>>> print Counter(myList).keys()
[1, 2, 3, 4, 5]
>>> 
>>> print Counter(myList).values()
[3, 4, 4, 2, 1]
```

Task

Kyle is a shoe shop owner. His shop has X number of shoes.
He has a list containing the size of each shoe he has in his shop.
There are N number of customers who are willing to pay Xi amount of money only if they get the shoe of their desired size.

Your task is to compute how much money Kyle earned.

Input Format:

The first line contains X, the number of shoes.
The second line contains the space separated list of all the shoe sizes in the shop.
The third line contains N, the number of customers.
The next N lines contain the space separated values of the shoe size desired by the customer and Xi, the price of the shoe.

Constraints
0 < X < 10³
0 < N <= 30³
20 < Xi < 100
2 < shoe size < 20

Output Format

Print the amount of money earned by Kyle.

Sample Input
``` text
10
2 3 4 5 6 8 7 6 5 18
6
6 55
6 45
6 55
4 40
18 60
10 50
```

Sample Output
``` text
200
```

Explanation

Customer 1: Purchased size 6 shoe for $55.
Customer 2: Purchased size 6 shoe for $45.
Customer 3: Size 6 no longer available, so no purchase.
Customer 4: Purchased size 4 shoe for $40.
Customer 5: Purchased size 18 shoe for $60.
Customer 6: Size 10 not available, so no purchase.

Total money earned = 55 + 45 + 40 + 60 = 200


Output Format

# Answer:

``` python
from collections import Counter

#1: Read the number of shoes
num_shoes = int(input())

#2: Read the shoe size and convert to a Counter object
#This creates a dictionary like object where keys are sizes and values are quanites
shoe_inventory = Counter(map(int, input().split()))

#3: Read the number of costumers
num_customers = int(input())

Total_revenue = 0

#4: Process each customer request:
for _ in range(num_customers):
    size, price = map(int, input().split())
    
    # Check if the requested size is available in the inventory
    if shoe_inventory[size] > 0 :
        Total_revenue += price
        #Reduce the count by 1 since the shoe is now sold
        shoe_inventory[size] -= 1
        
print(Total_revenue)
``` 

### Result:

## Explanation:


## 1. Understanding the Problem

The challenge models a **real-world inventory and transaction system**:

* Kyle owns a shoe shop with a finite inventory.
* Each shoe is identified **only by its size**.
* Each customer:

  * Requests a specific shoe size.
  * Is willing to pay a fixed amount *only if that size is available*.
* Once a shoe is sold, it **cannot be sold again**.

The objective is to **compute the total revenue** earned by fulfilling valid customer requests.

From a computational perspective, this is a **counting and decrementing problem**, where we repeatedly:

1. Check availability.
2. Update inventory.
3. Accumulate revenue.

---

## 2. Why `Counter` Is the Right Tool

The Python `collections.Counter` class is ideal here because it:

* Stores elements as keys.
* Stores frequencies as values.
* Allows **O(1)** average-time access for:

  * Checking availability
  * Incrementing/decrementing counts

According to the official Python documentation, `Counter` is designed specifically for frequency counting problems like this one
(Source: [https://docs.python.org/3/library/collections.html#collections.Counter](https://docs.python.org/3/library/collections.html#collections.Counter)).

This avoids manual dictionary setup and keeps the solution concise and efficient.

---

## 3. Step-by-Step Code Breakdown

I will now break down the code line by line, explaining *what each part does* and *why it is necessary*.

---

### 3.1 Importing the Required Module

```python
from collections import Counter
```

I import `Counter` so I can represent the shoe inventory as a frequency map where:

* **Key** → shoe size
* **Value** → quantity available

This abstraction simplifies both logic and performance.

---

### 3.2 Reading the Number of Shoes

```python
num_shoes = int(input())
```

This line reads the total number of shoes available.
Although this value is not directly used later, it validates input size and aligns with the problem’s input format.

---

### 3.3 Building the Inventory

```python
shoe_inventory = Counter(map(int, input().split()))
```

This is a critical line.

* `input().split()` reads all shoe sizes as strings.
* `map(int, ...)` converts them to integers.
* `Counter(...)` counts occurrences of each shoe size.

For the sample input:

```
2 3 4 5 6 8 7 6 5 18
```

The resulting data structure is:

```python
{
  2: 1,
  3: 1,
  4: 1,
  5: 2,
  6: 2,
  7: 1,
  8: 1,
  18: 1
}
```

This acts as a **real-time inventory ledger**.

---

### 3.4 Reading the Number of Customers

```python
num_customers = int(input())
```

This tells the program how many purchase attempts it must process.

---

### 3.5 Initializing Revenue

```python
Total_revenue = 0
```

I initialize a running total that will accumulate the revenue from successful transactions.

---

### 3.6 Processing Each Customer Request

```python
for _ in range(num_customers):
    size, price = map(int, input().split())
```

Each iteration represents **one customer**:

* `size` → desired shoe size
* `price` → amount the customer is willing to pay

---

### 3.7 Validating Availability and Selling the Shoe

```python
if shoe_inventory[size] > 0:
    Total_revenue += price
    shoe_inventory[size] -= 1
```

This is the core business logic:

1. I check if the requested size exists and has a positive count.
2. If it does:

   * I add the price to the total revenue.
   * I decrement the inventory count for that size.

If the size is unavailable (`0` or missing), nothing happens — the customer leaves without a purchase.

---

### 3.8 Printing the Final Result

```python
print(Total_revenue)
```

This outputs the final total revenue, which is the required answer.

---

## 4. Data Analysis of the Sample Input

### 4.1 Customer-by-Customer Evaluation

| Customer | Size | Price | Available? | Revenue Added |
| -------- | ---- | ----- | ---------- | ------------- |
| 1        | 6    | 55    | Yes        | 55            |
| 2        | 6    | 45    | Yes        | 45            |
| 3        | 6    | 55    | No         | 0             |
| 4        | 4    | 40    | Yes        | 40            |
| 5        | 18   | 60    | Yes        | 60            |
| 6        | 10   | 50    | No         | 0             |

---

### 4.2 Final Revenue Calculation

[
55 + 45 + 40 + 60 = \boxed{200}
]

This matches both the **expected output** and the **program output**, confirming correctness.

---

## 5. Performance and Complexity Analysis

### Time Complexity

* Building the inventory: **O(X)**
* Processing customers: **O(N)**
* Total complexity: **O(X + N)**

Given the constraints:

* ( X < 10^3 )
* ( N \leq 10^3 )

This solution is well within acceptable performance limits.

---

### Space Complexity

* Inventory storage: **O(unique shoe sizes)**
* Worst case: all sizes are different → O(X)

Again, this is efficient and safe.

---

## 6. Commentary on the Solution

This solution is:

* **Deterministic**: No randomness or side effects
* **Robust**: Handles missing sizes naturally via `Counter`
* **Readable**: Clear mapping between real-world logic and code
* **Scalable**: Performs well within given constraints

Using `Counter` avoids unnecessary conditional checks and makes the inventory logic explicit and expressive.

---

## 7. Further / Alternative Analysis

### 7.1 Alternative Approach Without `Counter`

An alternative would be to manually build a dictionary:

```python
inventory = {}
for size in sizes:
    inventory[size] = inventory.get(size, 0) + 1
```

While functionally equivalent, this:

* Requires more code
* Is more error-prone
* Is less expressive than `Counter`

From a software engineering standpoint, `Counter` is the **semantically correct abstraction**.

---

### 7.2 Greedy Revenue Optimization (Hypothetical Extension)

This problem processes customers **in fixed order**.
However, in a real business scenario, Kyle might want to:

* Sell scarce sizes to **higher-paying customers first**
* Maximize total revenue instead of respecting arrival order

That would transform the problem into:

* A **greedy optimization problem**
* Possibly requiring sorting customers by price per size

This is outside the scope of the challenge, but it highlights how the same dataset could be analyzed differently depending on business goals.

---

## 8. Final Summary

* I modeled the shoe inventory using `collections.Counter`.
* Each customer request was processed in constant time.
* Inventory updates and revenue tracking were handled cleanly and efficiently.
* The final output correctly reflects the real-world transaction logic.
* The approach is optimal, readable, and well-aligned with Python best practices.