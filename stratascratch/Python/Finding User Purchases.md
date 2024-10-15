## Challenge link: https://platform.stratascratch.com/coding/10322-finding-user-purchases?code_type=2

# Challenge: Finding User Purchases

![image](https://github.com/user-attachments/assets/663591a2-5c1c-4243-9d81-4c41a67f4409)

Write a query that'll identify returning active users. A returning active user is a user that has made a second purchase within 7 days of any other of their purchases. Output a list of user_ids of these returning active users.

![image](https://github.com/user-attachments/assets/6b69cc9f-aa8c-4b57-9987-efe0ea631a02)


# Answer:

``` python
import pandas as pd

amazon_transactions = amazon_transactions.sort_values(by=['user_id', 'created_at'])

amazon_transactions['prev_purchase_date'] = amazon_transactions.groupby('user_id')['created_at'].shift(1)

amazon_transactions['dates_diff'] = (amazon_transactions['created_at'] - amazon_transactions['prev_purchase_date']).dt.days

active_customers = amazon_transactions[(amazon_transactions['dates_diff'] <= 7)]

active_customers = active_customers['user_id'].drop_duplicates()

active_customers
``` 

### Result:

![image](https://github.com/user-attachments/assets/e5773b19-424d-4174-9242-e32231622721)


## Explanation:

To solve this challenge, we need to identify returning active users—those who made a second purchase within seven days of any other purchase they made. Here's a detailed breakdown of the solution and the code used to accomplish it.

### Step-by-Step Breakdown of the Code

1. **Import Required Libraries**:
   ```python
   import pandas as pd
   ```
   The `pandas` library is imported to enable data manipulation and analysis, especially for handling and analyzing the transaction data.

2. **Sort the Data**:
   ```python
   amazon_transactions = amazon_transactions.sort_values(by=['user_id', 'created_at'])
   ```
   Here, the data is sorted by `user_id` and `created_at` to ensure that each user’s transactions are ordered chronologically. This step is crucial because it allows us to accurately calculate the time difference between consecutive purchases for each user.

3. **Shift the Purchase Dates**:
   ```python
   amazon_transactions['prev_purchase_date'] = amazon_transactions.groupby('user_id')['created_at'].shift(1)
   ```
   The `shift(1)` function is applied within each user group to create a new column, `prev_purchase_date`, which holds the date of the user's previous purchase. This transformation helps in calculating the time difference between the current purchase and the previous purchase for each user.

4. **Calculate Date Difference**:
   ```python
   amazon_transactions['dates_diff'] = (amazon_transactions['created_at'] - amazon_transactions['prev_purchase_date']).dt.days
   ```
   Here, the time difference (in days) between the current purchase date (`created_at`) and the previous purchase date (`prev_purchase_date`) is calculated. This difference is stored in a new column, `dates_diff`. The `.dt.days` part converts the timedelta to an integer value representing days.

5. **Filter Returning Active Users**:
   ```python
   active_customers = amazon_transactions[(amazon_transactions['dates_diff'] <= 7)]
   ```
   Using a conditional filter, we keep only the rows where the `dates_diff` is less than or equal to 7 days. This identifies all purchases that were made within seven days of a previous purchase, hence indicating returning active users.

6. **Extract Unique User IDs**:
   ```python
   active_customers = active_customers['user_id'].drop_duplicates()
   ```
   Finally, the `user_id` column is extracted from the filtered data and `drop_duplicates()` is applied to obtain unique user IDs. This gives the list of user IDs of returning active users.

### Data Analysis of the Result

The result is a list of unique user IDs:
```
user_id
100
103
105
109
110
111
112
114
117
120
122
128
129
130
131
133
141
143
150
```

These user IDs represent the returning active users who made additional purchases within seven days of a previous purchase. The data shows that there are 19 users who fit this criterion.

- **Observations**:
  - The user activity suggests a subset of users who are more engaged, as they have returned to make a purchase within a short timeframe.
  - The limited number of returning active users (19) could indicate a relatively low retention rate if the total user base is large, or a high retention rate if the user base is small.

### Comment on the Solution

The solution efficiently identifies returning active users by leveraging time-based grouping and difference calculations, which is crucial for many real-world scenarios where understanding user engagement and retention is important. The methodology is straightforward and easy to interpret, as it combines basic operations like sorting, grouping, shifting, and filtering.

### Further or Alternative Analysis

To expand the analysis, we could explore the purchasing behavior by looking at:

1. **Frequency of Purchases within Different Time Windows**:
   - For instance, we could identify users who made purchases within 3 days, 7 days, and 14 days, providing more insight into engagement levels at different intervals.

2. **User Segmentation Based on Purchase Frequency**:
   - We could calculate the number of purchases each user made within the last month and categorize users into high, medium, and low-frequency segments.

3. **Analysis of Purchase Patterns for Returning Users**:
   - We could go beyond just identifying returning users and analyze specific products or categories they purchase. This would help in identifying trends or patterns in returning users' purchasing behavior.

#### Example of an Extended Analysis Code

Here’s an alternative approach to segment users based on 3-day, 7-day, and 14-day return periods:

```python
# Calculate frequency of purchases within 3, 7, and 14 days for each user
amazon_transactions['3_day_return'] = amazon_transactions['dates_diff'] <= 3
amazon_transactions['7_day_return'] = amazon_transactions['dates_diff'] <= 7
amazon_transactions['14_day_return'] = amazon_transactions['dates_diff'] <= 14

# Aggregate and analyze the data
return_analysis = amazon_transactions.groupby('user_id').agg(
    within_3_days=('3_day_return', 'sum'),
    within_7_days=('7_day_return', 'sum'),
    within_14_days=('14_day_return', 'sum')
).reset_index()

return_analysis.head()
```

This alternative analysis could help in understanding different levels of user engagement and could be instrumental in tailoring marketing efforts or customer loyalty programs based on user behavior. By evaluating multiple timeframes, we could discern patterns such as users who consistently make purchases within very short windows and those who take longer but still return frequently.
