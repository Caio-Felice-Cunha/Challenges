## Challenge link: https://platform.stratascratch.com/coding/10351-activity-rank?code_type=2

# Challenge: Patient Support Analysis (Part 2)
UnitedHealth Group (UHG) has a program called Advocate4Me, which allows policy holders (or, members) to call an advocate and receive support for their health care needs – whether that's claims and benefits support, drug coverage, pre- and post-authorisation, medical records, emergency assistance, or member portal services.

Calls to the Advocate4Me call centre are classified into various categories, but some calls cannot be neatly categorised. These uncategorised calls are labeled as “n/a”, or are left empty when the support agent does not enter anything into the call category field.

Write a query to calculate the percentage of calls that cannot be categorised. Round your answer to 1 decimal place. For example, 45.0, 48.5, 57.7.
![image](https://github.com/user-attachments/assets/6037261e-4e21-43b3-ad5b-90974a283557)


# Answer:

``` sql
SELECT 
  ROUND(
    COUNT(CASE 
      WHEN call_category = 'n/a' OR call_category IS NULL THEN 1 
    END) * 100.0 / COUNT(*), 1
  ) AS uncategorised_call_pct
FROM 
  callers;
``` 

### Result:
![image](https://github.com/user-attachments/assets/19aca608-ac03-4002-8f95-80b3bed9275b)


## Explanation:
### Key Components of the Query

1. **SELECT Statement**:
   - The query starts with a `SELECT` statement, which is used to specify the data we want to retrieve from the database. In this case, we are calculating the percentage of uncategorized calls.

2. **ROUND Function**:
   - The `ROUND` function is used to round the resulting percentage to one decimal place. This ensures the output is user-friendly and meets the requirements.

3. **COUNT with CASE Statement**:
   - The `COUNT` function is used to count the number of rows that meet a certain condition. Here, a `CASE` statement is utilized to specify the condition:
     - `WHEN call_category = 'n/a' OR call_category IS NULL THEN 1`:
       - This checks if the `call_category` is either "n/a" or `NULL`. If true, it counts that row (returns 1).
       - Rows that do not meet this condition will not contribute to the count.

4. **COUNT(*)**:
   - This counts all rows in the `callers` table. This serves as the denominator in our percentage calculation.

5. **Percentage Calculation**:
   - The formula `COUNT(CASE ...) * 100.0 / COUNT(*)` calculates the percentage of uncategorized calls. The multiplication by `100.0` ensures that the result is a floating-point number rather than an integer, maintaining precision during division.

6. **FROM Clause**:
   - `FROM callers;` specifies that the data is being retrieved from the `callers` table.

### Final Output
- The query outputs a single column named `uncategorised_call_pct`, which represents the percentage of calls that cannot be categorized.

## Data Analysis of the Result

### Result Interpretation
- The result of the query is:
  ```
  uncategorised_call_pct
  -----------------------
  5.5
  ```

### Meaning of the Result
- The output indicates that **5.5%** of all calls to the Advocate4Me call center are categorized as "n/a" or left empty by the support agents. This figure suggests a relatively small portion of the calls are unclassified, which can be interpreted in various ways:

1. **Efficiency of the Call Center**:
   - A low percentage of uncategorized calls might indicate that most calls are successfully classified, reflecting effective training and categorization processes among the support agents.

2. **Potential Improvement Areas**:
   - Despite the low percentage, 5.5% of calls still represent a notable number of interactions. This uncategorized segment could represent areas where additional training or better categorization guidelines might be beneficial.

3. **Customer Experience**:
   - Calls that cannot be categorized might lead to confusion or dissatisfaction among members if their needs are not clearly identified. Understanding the nature of these uncategorized calls could improve overall customer satisfaction.

## Further or Alternative Analysis

### Suggestions for Additional Analysis
1. **Categorization Analysis**:
   - Conduct a deeper analysis into the nature of the calls that are categorized as "n/a" or left empty. This could include reviewing the content of these calls (if possible) to understand common themes or issues.
   - For example, if many uncategorized calls involve complex queries, additional training on these topics for support agents might be warranted.

2. **Time Series Analysis**:
   - Analyze trends over time to see if the percentage of uncategorized calls is decreasing, stable, or increasing. This can provide insight into the effectiveness of any initiatives taken to reduce the number of uncategorized calls.
   - By comparing monthly or quarterly data, the organization can assess if recent changes have had a positive impact.

3. **Comparison with Other Call Centers**:
   - Compare this percentage with industry benchmarks or with other call centers within UHG. A higher-than-average percentage could indicate specific issues in this center that need addressing.

4. **Root Cause Analysis**:
   - Explore the reasons behind the uncategorized calls. Are they due to agent error, complex customer inquiries, or issues with the call categorization system?
   - By employing qualitative methods (like listening to call recordings or customer feedback), UHG can better understand the challenges agents face and develop targeted training programs.

5. **Impact on Overall Call Center Metrics**:
   - Assess how uncategorized calls impact other metrics, such as average call resolution time, customer satisfaction scores, or the number of follow-up calls needed. This can provide a holistic view of the effect of uncategorized calls on overall service delivery.

### Conclusion
In conclusion, while the query provides a useful metric for assessing call categorization effectiveness, additional analysis could yield valuable insights that inform training, improve categorization processes, and ultimately enhance the member experience with the Advocate4Me program. Addressing the issue of uncategorized calls can lead to better service delivery and greater customer satisfaction.
