## Challenge link:

# Challenge: Apple Product Counts

![image](https://github.com/user-attachments/assets/a203e480-62b3-4bfb-b065-daea6a87e51d)

Find the number of Apple product users and the number of total users with a device and group the counts by language. Assume Apple products are only MacBook-Pro, iPhone 5s, and iPad-air. Output the language along with the total number of Apple users and users with any device. Order your results based on the number of total users in descending order.

![image](https://github.com/user-attachments/assets/cc5367e3-edbb-4cce-935c-cb6c906d3182) ![image](https://github.com/user-attachments/assets/d6c1639f-aeb1-411b-92da-76ea9c9e6cfe)

# Answer:

``` sql
SELECT 
    playbook_users.language
    , COUNT(DISTINCT CASE WHEN 
        playbook_events.device IN ('macbook pro','iphone 5s','ipad air')
        THEN playbook_events.user_id
        ELSE NULL
    END) AS Apple_Users
    , COUNT(DISTINCT playbook_events.user_id) AS Total_Users
FROM 
    playbook_events
JOIN 
    playbook_users ON playbook_events.user_id = playbook_users.user_id
GROUP BY 
    playbook_users.language
ORDER BY 
    Total_Users DESC;
``` 
![image](https://github.com/user-attachments/assets/44ab4cd8-248c-4616-a22e-38b0e82dcc64)

## Explanation:
### Problem Breakdown

The challenge involves calculating the number of Apple product users and the total number of users with any device, grouped by language. The assumption is that Apple products include only "MacBook-Pro," "iPhone 5s," and "iPad-air."

### Solution Explanation

Here's the SQL query provided to solve this challenge, along with a detailed breakdown of each part:

```sql
SELECT 
    playbook_users.language,
    COUNT(DISTINCT CASE WHEN 
        playbook_events.device IN ('macbook pro', 'iphone 5s', 'ipad air')
        THEN playbook_events.user_id
        ELSE NULL
    END) AS Apple_Users,
    COUNT(DISTINCT playbook_events.user_id) AS Total_Users
FROM 
    playbook_events
JOIN 
    playbook_users ON playbook_events.user_id = playbook_users.user_id
GROUP BY 
    playbook_users.language
ORDER BY 
    Apple_Users DESC;
```

#### Step-by-Step Breakdown

1. **`SELECT` Clause**:
   - `playbook_users.language`: This selects the language of the users.
   - `COUNT(DISTINCT CASE WHEN ...) AS Apple_Users`: This part counts the distinct users who have an Apple device. The `CASE WHEN` statement checks if the `device` is one of the specified Apple products. If true, it returns the `user_id`, which is then counted distinctly.
   - `COUNT(DISTINCT playbook_events.user_id) AS Total_Users`: This counts all distinct users who have any device, regardless of whether it's an Apple product or not.

2. **`FROM` Clause**:
   - `playbook_events`: This table presumably contains records of events, including information about the device used and the user who performed the event.

3. **`JOIN` Clause**:
   - `JOIN playbook_users ON playbook_events.user_id = playbook_users.user_id`: This joins the `playbook_users` table with the `playbook_events` table on `user_id`. The `playbook_users` table likely contains user-related information, such as language.

4. **`GROUP BY` Clause**:
   - `GROUP BY playbook_users.language`: This groups the results by the language of the users.

5. **`ORDER BY` Clause**:
   - `ORDER BY Apple_Users DESC`: This orders the final results by the number of Apple users in descending order.

### Alternative Solution

The given solution is efficient and solves the problem correctly, but there's a slight variation that could be considered:

```sql
SELECT 
    playbook_users.language,
    COUNT(DISTINCT CASE WHEN 
        playbook_events.device IN ('macbook pro', 'iphone 5s', 'ipad air')
        THEN playbook_events.user_id
    END) AS Apple_Users,
    COUNT(DISTINCT playbook_events.user_id) AS Total_Users
FROM 
    playbook_events
JOIN 
    playbook_users ON playbook_events.user_id = playbook_users.user_id
GROUP BY 
    playbook_users.language
ORDER BY 
    Total_Users DESC;
```

**Key Difference:**
- The results are now ordered by `Total_Users` instead of `Apple_Users`. This allows us to see the languages with the most users overall, rather than just those with the most Apple users.

### Data Analysis and Comments

#### Results Interpretation

The results show the number of Apple users and total users grouped by language:

| language  | Apple_Users | Total_Users |
|-----------|-------------|-------------|
| english   | 11          | 45          |
| spanish   | 3           | 9           |
| japanese  | 2           | 6           |
| chinese   | 1           | 4           |
| german    | 1           | 3           |
| italian   | 1           | 1           |
| portugese | 1           | 3           |
| arabic    | 0           | 2           |
| french    | 0           | 5           |
| indian    | 0           | 2           |
| russian   | 0           | 5           |

**Observations**:
- **English**-speaking users dominate both in terms of total users and Apple users. This suggests that the English-speaking user base is more inclined towards using Apple products.
- **Spanish** and **Japanese**-speaking users have a moderate presence, with Spanish users having more Apple users compared to Japanese users.
- There are languages like **Italian** where all users have Apple products, while languages like **Arabic** and **French** have none.
- **Russian** and **French** have no Apple users despite having a reasonable number of total users, which might indicate a preference for non-Apple devices in these regions.

### Additional Analysis

A further analysis could involve calculating the percentage of Apple users among total users for each language:

```sql
SELECT 
    playbook_users.language,
    COUNT(DISTINCT CASE WHEN 
        playbook_events.device IN ('macbook pro', 'iphone 5s', 'ipad air')
        THEN playbook_events.user_id
    END) AS Apple_Users,
    COUNT(DISTINCT playbook_events.user_id) AS Total_Users,
    COUNT(DISTINCT CASE WHEN 
        playbook_events.device IN ('macbook pro', 'iphone 5s', 'ipad air')
        THEN playbook_events.user_id
    END) * 100.0 / COUNT(DISTINCT playbook_events.user_id) AS Apple_Users_Percentage
FROM 
    playbook_events
JOIN 
    playbook_users ON playbook_events.user_id = playbook_users.user_id
GROUP BY 
    playbook_users.language
ORDER BY 
    Apple_Users_Percentage DESC;
```

**Additional Comments**:
- The percentage metric would provide insights into the penetration of Apple products within each language group.
- This could be useful for marketing strategies or understanding regional preferences for product development.

Overall, the initial solution is solid, and these variations offer additional perspectives depending on the analysis focus.
