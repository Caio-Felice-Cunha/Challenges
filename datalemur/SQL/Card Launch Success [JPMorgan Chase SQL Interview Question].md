## Challenge link: https://datalemur.com/questions/card-launch-success

# Challenge: Card Launch Success [JPMorgan Chase SQL Interview Question]

Your team at JPMorgan Chase is soon launching a new credit card. You are asked to estimate how many cards you'll issue in the first month.

Before you can answer this question, you want to first get some perspective on how well new credit card launches typically do in their first month.

Write a query that outputs the name of the credit card, and how many cards were issued in its launch month. The launch month is the earliest record in the monthly_cards_issued table for a given card. Order the results starting from the biggest issued amount.

![image](https://github.com/user-attachments/assets/de11e412-fae4-456a-b367-c0a7f621aefd)



# Answer:

``` sql
SELECT 
  card_name 
  , issued_amount
FROM 
  monthly_cards_issued AS mci
WHERE 
  MAKE_DATE(issue_year, issue_month, 1) = 
    (
      SELECT 
        MIN(
          MAKE_DATE(issue_year, issue_month, 1))
      FROM 
        monthly_cards_issued
      WHERE 
        card_name = mci.card_name
  )
ORDER BY 
  issued_amount DESC;

``` 

### Result:
![image](https://github.com/user-attachments/assets/97f26555-a19b-4142-af99-721c67ebea7f)

## Explanation:
Let's break down the solution step by step, analyze the result, and then discuss possible further or alternative analyses.

### Problem Statement
The goal is to estimate how many credit cards will be issued in the first month of a new card launch. To do this, we need to examine how many cards were issued during the launch month of previous credit cards and sort them by the number of cards issued.

### Breakdown of the SQL Query

```sql
SELECT 
  card_name, 
  issued_amount
FROM 
  monthly_cards_issued AS mci
WHERE 
  MAKE_DATE(issue_year, issue_month, 1) = 
    (
      SELECT 
        MIN(
          MAKE_DATE(issue_year, issue_month, 1))
      FROM 
        monthly_cards_issued
      WHERE 
        card_name = mci.card_name
  )
ORDER BY 
  issued_amount DESC;
```

#### 1. **Selecting Columns:**
```sql
SELECT 
  card_name, 
  issued_amount
```
- **card_name:** The name of the credit card.
- **issued_amount:** The number of cards issued in the specified month.

#### 2. **Table and Alias:**
```sql
FROM 
  monthly_cards_issued AS mci
```
- We are selecting from the `monthly_cards_issued` table, and `mci` is used as an alias for easier reference later in the query.

#### 3. **Filtering to Find the Launch Month:**
```sql
WHERE 
  MAKE_DATE(issue_year, issue_month, 1) = 
    (
      SELECT 
        MIN(
          MAKE_DATE(issue_year, issue_month, 1))
      FROM 
        monthly_cards_issued
      WHERE 
        card_name = mci.card_name
  )
```
- **MAKE_DATE(issue_year, issue_month, 1):** Converts the `issue_year` and `issue_month` into a date format with the day set as the first of the month. This helps in handling dates more effectively.
- The subquery is used to find the **earliest date** for each card by using `MIN(MAKE_DATE(issue_year, issue_month, 1))`. This ensures that we only select the records from the launch month.
- The `WHERE` clause inside the subquery ensures that we are looking at the earliest date for the same `card_name`.

#### 4. **Ordering the Results:**
```sql
ORDER BY 
  issued_amount DESC;
```
- The results are ordered by `issued_amount` in descending order so that the most successful card launches (in terms of the number of cards issued) appear first.

### Data Analysis of the Result

| card_name              | issued_amount |
|------------------------|---------------|
| Chase Sapphire Reserve | 150,000       |
| Chase Freedom Flex     | 55,000        |

#### Analysis:
- **Chase Sapphire Reserve:** This card had a significantly successful launch with 150,000 cards issued in the first month. It suggests that this card had strong market demand or was heavily promoted.
- **Chase Freedom Flex:** While less than the Sapphire Reserve, 55,000 cards is still a strong showing, indicating a successful but more moderate launch.

### Comment on the Results
The difference in the number of issued cards suggests varying levels of market interest, possibly due to different target audiences, card features, or marketing efforts. The large disparity between the two numbers could indicate that factors such as brand prestige, rewards structure, or marketing strategy significantly influence the success of a card launch.

### Further or Alternative Analysis

1. **Analyzing Success Factors:**
   - We could investigate what differentiates these successful launches. This might include additional queries to explore metrics such as the rewards offered, fees, or the demographics of the users who applied for these cards.
   - Example query:
   ```sql
   SELECT 
     card_name, 
     AVG(reward_points), 
     AVG(fees)
   FROM 
     card_details
   WHERE 
     card_name IN ('Chase Sapphire Reserve', 'Chase Freedom Flex')
   GROUP BY 
     card_name;
   ```
   - **Purpose:** Understanding whether higher rewards or lower fees correlate with higher launch success.

2. **Trend Analysis Over Time:**
   - We can analyze the trend of card issuance over time to see if there's a consistent pattern that can predict the success of future launches.
   - Example query:
   ```sql
   SELECT 
     issue_year, 
     issue_month, 
     SUM(issued_amount) AS total_issued
   FROM 
     monthly_cards_issued
   GROUP BY 
     issue_year, issue_month
   ORDER BY 
     issue_year, issue_month;
   ```
   - **Purpose:** Identify any seasonal trends or changes in consumer behavior over time.

3. **Comparative Analysis of Card Features:**
   - Compare the features of cards that had different levels of success to see which features might correlate with higher issuance.
   - Example query:
   ```sql
   SELECT 
     card_name, 
     feature, 
     COUNT(*) AS feature_count
   FROM 
     card_features
   WHERE 
     card_name IN ('Chase Sapphire Reserve', 'Chase Freedom Flex')
   GROUP BY 
     card_name, feature
   ORDER BY 
     feature_count DESC;
   ```
   - **Purpose:** Determine if certain features (e.g., travel rewards, cash back) are more associated with higher issuance numbers.

### Conclusion
The initial analysis highlights the significance of card launch success and how it can vary widely. By examining factors like rewards, fees, or seasonal trends, one could refine the analysis to make better predictions for future launches. This deeper understanding can help tailor strategies to maximize the success of upcoming credit card releases.
