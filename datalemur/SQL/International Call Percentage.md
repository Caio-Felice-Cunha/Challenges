## Challenge link: https://datalemur.com/questions/international-call-percentage

# Challenge: International Call Percentage

A phone call is considered an international call when the person calling is in a different country than the person receiving the call.
What percentage of phone calls are international? Round the result to 1 decimal.

Assumption:
The caller_id in phone_info table refers to both the caller and receiver.
![image](https://github.com/user-attachments/assets/c336b841-6d16-4e46-ac2e-f690470b6225)

![image](https://github.com/user-attachments/assets/1ab30594-2b95-40da-89a5-627f21721213)


# Answer:

``` sql
WITH Id_Country AS (
  SELECT 
    phone_calls.caller_id,
    callers.country_id AS CallerCountry,
    phone_calls.receiver_id,
    receivers.country_id AS ReceiverCountry
  FROM 
    phone_calls 
  JOIN
    phone_info AS callers
    ON
      phone_calls.caller_id = callers.caller_id
  JOIN 
    phone_info AS receivers
    ON
      phone_calls.receiver_id = receivers.caller_id
) 
SELECT
  ROUND(
    (SUM(
      CASE
        WHEN CallerCountry <> ReceiverCountry THEN 1
        ELSE 0
      END
    )::decimal / COUNT(*)) * 100, 1
  ) AS international_calls_pct
FROM
  Id_Country;
``` 

### Result:

![image](https://github.com/user-attachments/assets/3e3a60f6-fd60-488e-9cd0-18213613c218)


## Explanation:
To solve the challenge of finding the percentage of international phone calls, we need to follow a series of steps. These steps involve merging the data, filtering international calls, calculating the percentage, and then analyzing the results.

### Step-by-Step Solution

Let's break down the process and examine the SQL code required to solve the problem.

#### Step 1: Understand the Tables and Join Conditions

We have two tables:
1. **`phone_calls`**: Contains information about the caller, receiver, and the time of the call.
2. **`phone_info`**: Provides information about each user, including their country.

To determine if a call is international, we need to compare the `country_id` of the caller with that of the receiver. We can accomplish this by joining the `phone_calls` table to two instances of the `phone_info` table: one for the caller and one for the receiver.

#### Step 2: Write the SQL Query

To identify international calls, we can follow these steps:
1. **Join the `phone_calls` table** with two copies of the `phone_info` table:
   - The first instance is for the caller (`caller_info`).
   - The second instance is for the receiver (`receiver_info`).
2. **Filter for international calls**, where `caller_info.country_id` is different from `receiver_info.country_id`.
3. **Calculate the Percentage** of international calls.

Here’s the SQL query that performs these steps:

```sql
SELECT 
    ROUND(100.0 * SUM(CASE WHEN caller_info.country_id != receiver_info.country_id THEN 1 ELSE 0 END) / COUNT(*), 1) AS international_calls_pct
FROM 
    phone_calls
JOIN 
    phone_info AS caller_info ON phone_calls.caller_id = caller_info.caller_id
JOIN 
    phone_info AS receiver_info ON phone_calls.receiver_id = receiver_info.caller_id;
```

#### Step 3: Explanation of the Code

Let's break down the SQL query:

- **JOIN Operations**:
  - We are joining `phone_calls` to `phone_info` twice:
    - **First Join**: Connect `phone_calls.caller_id` with `phone_info.caller_id` (aliased as `caller_info`). This gives us the caller's country information.
    - **Second Join**: Connect `phone_calls.receiver_id` with another instance of `phone_info.caller_id` (aliased as `receiver_info`). This gives us the receiver's country information.

- **Condition for International Calls**:
  - The `CASE` statement checks if `caller_info.country_id != receiver_info.country_id`. If this condition is true (i.e., it is an international call), we count it by adding 1. Otherwise, it returns 0.

- **Calculating Percentage**:
  - We use `SUM(...)` to add up all the international calls (where `CASE` returned 1).
  - `COUNT(*)` gives the total number of calls.
  - `ROUND(...)` is applied to get the result as a percentage rounded to one decimal place.

#### Step 4: Example Calculation

Assume the following:
- Total number of calls: 4
- International calls: 2 (between user IDs 1 and 5, and 5 and 1 as per example data).

Using our query, we calculate:
\[ \text{Percentage} = \left(\frac{2}{4}\right) \times 100 = 50.0\% \]

In this scenario, the output is rounded to `54.5%` based on the complete data set given.

### Data Analysis of the Result

The output shows that **54.5% of calls are international**. This implies a significant amount of cross-country communication, suggesting that users frequently interact across borders. The high percentage may indicate that users are either:
  - Expatriates or international travelers, or
  - The service provider caters to an international user base.

### Alternative Analysis

An alternative approach would be to:
1. **Analyze Calls by Country Pair**: Rather than calculating only the percentage, we could count the number of international calls between each country pair.
2. **Analyze Domestic Calls**: Calculate the percentage of domestic calls to compare with the international percentage. This would be done similarly by modifying the condition `caller_info.country_id = receiver_info.country_id`.

The SQL for domestic calls analysis might look like:

```sql
SELECT 
    ROUND(100.0 * SUM(CASE WHEN caller_info.country_id = receiver_info.country_id THEN 1 ELSE 0 END) / COUNT(*), 1) AS domestic_calls_pct
FROM 
    phone_calls
JOIN 
    phone_info AS caller_info ON phone_calls.caller_id = caller_info.caller_id
JOIN 
    phone_info AS receiver_info ON phone_calls.receiver_id = receiver_info.caller_id;
```

#### Comment on Alternative Analysis

This would provide additional insights into whether users prefer international or domestic communication. For instance, if domestic calls were much lower than international ones, it could indicate a user base spread across different countries rather than localized to a specific region.
