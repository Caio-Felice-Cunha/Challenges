## Challenge link: https://platform.stratascratch.com/coding/10351-activity-rank?code_type=2

# Challenge: Activity Rank
![image](https://github.com/user-attachments/assets/5fb3554a-635e-4818-8a42-30a4b7edd101)

Find the email activity rank for each user. Email activity rank is defined by the total number of emails sent. The user with the highest number of emails sent will have a rank of 1, and so on. Output the user, total emails, and their activity rank. Order records by the total emails in descending order. Sort users with the same number of emails in alphabetical order.
In your rankings, return a unique value (i.e., a unique rank) even if multiple users have the same number of emails. For tie breaker use alphabetical order of the user usernames.

![image](https://github.com/user-attachments/assets/9b792898-f459-40ab-80e8-937b24fbf136)


# Answer:

``` python
# Import your libraries
import pandas as pd

# Start writing code
google_gmail_emails.head()

emails_counts = google_gmail_emails.groupby('from_user').size().reset_index(name='total_emails')

emails_counts = emails_counts.sort_values(by=['total_emails','from_user'], ascending=[False, True])

emails_counts['activity_rank'] = range(1, len(emails_counts) + 1)

emails_counts
``` 

### Result:
![image](https://github.com/user-attachments/assets/ad2bc535-290f-434f-805d-5fcbd2249495)


## Explanation:

Let's go through the code step-by-step to understand the solution, analyze the data, and discuss further analysis options.

### Step-by-Step Explanation

1. **Importing Libraries**
   ```python
   import pandas as pd
   ```
   The code starts by importing the `pandas` library, which is essential for data manipulation and analysis in Python. `pandas` provides powerful data structures like DataFrames that simplify handling large datasets.

2. **Displaying the Data**
   ```python
   google_gmail_emails.head()
   ```
   This line would display the first few rows of the `google_gmail_emails` DataFrame. It helps to preview the data and ensure the table's structure, but it’s mostly for exploratory purposes and not essential to the final solution.

3. **Grouping and Counting Emails Sent by Each User**
   ```python
   emails_counts = google_gmail_emails.groupby('from_user').size().reset_index(name='total_emails')
   ```
   - **Grouping**: The code groups the data by the `from_user` column, where each row represents an email sent by that user.
   - **Counting**: The `.size()` method counts the number of emails for each user, resulting in the total email count per user.
   - **Resetting the Index**: `reset_index(name='total_emails')` flattens the grouped result into a DataFrame and renames the count column to `total_emails`.

4. **Sorting by Email Count and User**
   ```python
   emails_counts = emails_counts.sort_values(by=['total_emails','from_user'], ascending=[False, True])
   ```
   - The data is sorted by `total_emails` in descending order (high to low), so the user with the most emails appears first.
   - The `from_user` column is sorted in ascending alphabetical order to handle any ties (where two or more users have the same `total_emails` count). This sorting ensures that users with the same email count are ranked alphabetically.

5. **Assigning Rank to Each User**
   ```python
   emails_counts['activity_rank'] = range(1, len(emails_counts) + 1)
   ```
   - This line assigns a unique rank to each user based on their position after sorting.
   - `range(1, len(emails_counts) + 1)` generates a list of ranks starting from 1 up to the total number of users.
   - A new column `activity_rank` is added to the DataFrame, containing each user’s rank.

6. **Displaying the Result**
   ```python
   emails_counts
   ```
   This displays the final DataFrame with columns for `from_user`, `total_emails`, and `activity_rank`.

### Data Analysis of the Result

The final DataFrame shows each user's email activity rank, ordered by the total number of emails sent, with ties broken alphabetically. Below are some insights and observations:

- **Top Users**: The user with the highest activity rank (`32ded68d89443e808`) sent 19 emails, followed by `ef5fe98c6b9f313075`, also with 19 emails. Their alphabetical order determines their rank (1 and 2, respectively).
- **Email Distribution**: The number of emails decreases gradually as the rank increases. This provides a clear view of the distribution and engagement levels across different users.
- **Ties in Email Counts**: Several users share the same email count, e.g., ranks 6 through 9 each sent 15 emails. The alphabetical order of user IDs resolves these ties.

### Comment on the Analysis

This solution effectively ranks users based on email activity, providing a clear picture of email engagement. Sorting ties alphabetically ensures consistent and unique ranking even when users have identical activity levels.

### Further or Alternative Analysis

#### Alternative Ranking Method
An alternative analysis could rank users using a **dense ranking** method, where users with the same number of emails share the same rank, and subsequent ranks are adjusted accordingly (i.e., no gaps in rank numbers). This would modify the ranking assignment step:

```python
emails_counts['activity_rank'] = emails_counts['total_emails'].rank(method='dense', ascending=False)
```
- This method would assign the same rank to users with equal email counts, but the next unique rank would follow immediately (e.g., two users with 19 emails would both be rank 1, and the next rank would be 2).

#### Visualizing Email Activity Distribution
A further analysis could include **visualizing the distribution** of total emails sent:
   - **Histogram**: To show the frequency of different email counts among users.
   - **Bar Chart**: For a more user-focused view, displaying total emails sent per user.
   - **Cumulative Distribution**: To understand the cumulative percentage of emails sent by top users, indicating if a small group of users accounts for a majority of the email activity.

These approaches offer deeper insights into the data distribution and help identify patterns, such as a few users dominating the email activity or more uniform participation across users.

