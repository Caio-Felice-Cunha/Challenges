## Challenge link: https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=true

# Challenge: The Report

You are given two tables: Students and Grades. Students contains three columns ID, Name and Marks.

![image](https://github.com/user-attachments/assets/5dcca205-de21-4db3-abd9-98e533008459)

Grades contains the following data:

![image](https://github.com/user-attachments/assets/12d6320c-c098-4c85-9d7d-fc4a94a2925e)

Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Write a query to help Eve.

# Answer:

``` sql
SELECT
    CASE
        WHEN Grades.Grade >= 8 THEN Students.Name
        ELSE NULL
        END
    , Grades.Grade
    , Students.Marks
FROM
    Students
JOIN
    Grades
    ON
        Students.Marks
        BETWEEN
            Grades.Min_Mark
            AND Grades.Max_Mark
ORDER BY
    Grades.Grade DESC
    , CASE 
        WHEN Grades.Grade >= 8 THEN Students.Name 
        ELSE Students.Marks 
        END 
    ASC
;
```

### **Result**

| Name      | Grade | Marks |
|-----------|-------|-------|
| Heraldo   | 10    | 94    |
| Julia     | 10    | 96    |
| Kristeen  | 10    | 100   |
| Stuart    | 10    | 99    |
| Amina     | 9     | 89    |
| Christene | 9     | 88    |
| Salma     | 9     | 81    |
| Samantha  | 9     | 87    |
| Scarlet   | 9     | 80    |
| Vivek     | 9     | 84    |
| Aamina    | 8     | 77    |
| Belvet    | 8     | 78    |
| Paige     | 8     | 74    |
| Priya     | 8     | 76    |
| Priyanka  | 8     | 77    |
| NULL      | 7     | 64    |
| NULL      | 7     | 66    |
| NULL      | 6     | 55    |
| NULL      | 4     | 34    |
| NULL      | 3     | 24    |   



## Explanation:

The challenge is to generate a report from two tables, `Students` and `Grades`. The report should include three columns: `Name`, `Grade`, and `Marks`. The requirements for the report are:

1. Only students who received a grade of 8 or higher should have their names listed in the report. For students with grades lower than 8, the name should be "NULL."
2. The report must be ordered in descending order of grades.
3. For students with the same grade:
   - If the grade is 8 or higher, the students should be ordered alphabetically by their names.
   - If the grade is lower than 8, students should be ordered by their marks in ascending order.

### Tables Structure:

- **Students**: This table contains the following columns:
  - `ID`: Unique identifier for each student.
  - `Name`: Name of the student.
  - `Marks`: The marks obtained by the student.

- **Grades**: This table contains the following columns:
  - `Grade`: The grade assigned based on a range of marks.
  - `Min_Mark`: The minimum mark required to achieve this grade.
  - `Max_Mark`: The maximum mark that can still achieve this grade.

### Solution Explanation:

The solution provided uses a `SELECT` query to extract the necessary data and meet the requirements of the challenge. Let's break down the solution:

```sql
SELECT
    CASE
        WHEN Grades.Grade >= 8 THEN Students.Name
        ELSE NULL
        END AS Name,
    Grades.Grade,
    Students.Marks
FROM
    Students
JOIN
    Grades ON Students.Marks BETWEEN Grades.Min_Mark AND Grades.Max_Mark
ORDER BY
    Grades.Grade DESC,
    CASE 
        WHEN Grades.Grade >= 8 THEN Students.Name 
        ELSE Students.Marks 
    END ASC;
```

#### 1. `SELECT` Clause:

- **CASE Statement for Name**:
  - The `CASE` statement is used to determine whether the student's name should be displayed or replaced with "NULL".
  - `WHEN Grades.Grade >= 8 THEN Students.Name ELSE NULL END AS Name`: If the grade is 8 or higher, the student's name is shown. Otherwise, "NULL" is displayed.

- **Grade and Marks**:
  - `Grades.Grade`: This column displays the grade for each student.
  - `Students.Marks`: This column displays the marks of the student.

#### 2. `FROM` and `JOIN` Clause:

- **JOIN Students with Grades**:
  - `JOIN Grades ON Students.Marks BETWEEN Grades.Min_Mark AND Grades.Max_Mark`: This line joins the `Students` table with the `Grades` table using the condition that the student's marks fall between the minimum and maximum marks defined for a particular grade.

#### 3. `ORDER BY` Clause:

- **Order by Grade**:
  - `Grades.Grade DESC`: The results are first ordered by grade in descending order, so higher grades appear first.

- **Order by Name or Marks**:
  - `CASE WHEN Grades.Grade >= 8 THEN Students.Name ELSE Students.Marks END ASC`: This line further orders the results:
    - If the grade is 8 or higher, the students are ordered alphabetically by their name.
    - If the grade is lower than 8, students are ordered by their marks in ascending order.

### Result Analysis:

The query produces a report with the following format:

- Students with grades 8 or higher are listed by name, with their grades in descending order and names sorted alphabetically if they share the same grade.
- Students with grades lower than 8 have their names replaced by "NULL" and are sorted by their marks in ascending order.

### Data Analysis and Comment:

#### Data Analysis:
- The top students in the report are those who scored high enough to achieve grades 8-10. These students are named and sorted by grade and then alphabetically within the same grade.
- Students with lower grades (1-7) are anonymized by replacing their names with "NULL" and sorted by their marks, indicating lower performance.

#### Comment:
- The approach used ensures clarity by separating high-performing students from those who did not meet the minimum grade threshold for naming. This anonymization of lower grades might be useful in contexts where it’s important to recognize top performers while still listing all students' results.
- The ordering by marks for students with grades below 8 highlights the relative performance within this group without drawing attention to specific individuals.

### Alternative Solution:

An alternative approach could use a Common Table Expression (CTE) to first filter and rank students before applying the `CASE` statement and ordering. This might offer better readability or flexibility for more complex requirements, but the logic remains fundamentally the same.

```sql
WITH RankedStudents AS (
    SELECT
        Students.Name,
        Grades.Grade,
        Students.Marks,
        ROW_NUMBER() OVER (PARTITION BY Grades.Grade ORDER BY 
            CASE WHEN Grades.Grade >= 8 THEN Students.Name ELSE Students.Marks END ASC) AS StudentRank
    FROM
        Students
    JOIN
        Grades ON Students.Marks BETWEEN Grades.Min_Mark AND Grades.Max_Mark
)
SELECT
    CASE
        WHEN Grade >= 8 THEN Name
        ELSE NULL
    END AS Name,
    Grade,
    Marks
FROM
    RankedStudents
ORDER BY
    Grade DESC,
    StudentRank ASC;
```

This alternative solution achieves the same result but introduces a ranking system via `ROW_NUMBER()` for better clarity when ordering the results.
