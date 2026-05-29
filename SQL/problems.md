You are given two tables: Students and Grades. Students contains three columns ID, Name and Marks.
<img width="300" height="167" alt="Screenshot 2026-05-29 at 07 45 08" src="https://github.com/user-attachments/assets/25aa044e-1481-44bf-9eaa-7a276bd61b15" />
Grades contains the following data:
<img width="304" height="458" alt="Screenshot 2026-05-29 at 07 45 27" src="https://github.com/user-attachments/assets/5f941481-1c41-4a08-8ef1-b9f8e1bcbbee" />
Ketty gives Eve a task to generate a report containing three columns: 
Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. 
The report must be in descending order by grade -- i.e. higher grades are entered first. 
If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. 
Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. 
If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.
Write a query to help Eve.

Answer:
SELECT 
    CASE
        WHEN g.Grade < 8 THEN 'NULL'
        ELSE s.Name
    END,
    g.Grade,
    s.Marks
FROM Students s
JOIN Grades g ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
ORDER BY
    g.Grade DESC,
    CASE WHEN g.Grade >= 8 THEN s.Name ELSE NULL END ASC,
    CASE WHEN g.Grade < 8 THEN s.Marks ELSE NULL END ASC;

Report:
Required a non-equi JOIN (matching stundet marks to a grade range using BETWEEN).
Combined with conditional sorting via CASE expressions in ORDER BY.

Note:
First attempt failed with `SQL0204N` on HackerRank's DB2 environment despite working locally on PostgreSQL.
Switching the platform language to MySQL resolved it.
My first encounter with SQL dialect differences in practice.

----


