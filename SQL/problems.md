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

------

Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to  decimal places.

<img width="602" height="705" alt="Screenshot 2026-05-31 at 20 12 45" src="https://github.com/user-attachments/assets/da7ea68d-e3ea-4aea-93f0-05f171db70d1" />


Answer:

SELECT ROUND(s1.LAT_N, 4)
FROM STATION s1
WHERE (
    SELECT COUNT(*) 
    FROM STATION s2 
    WHERE s2.LAT_N <= s1.LAT_N
) = (
    SELECT COUNT(*) 
    FROM STATION s3 
    WHERE s3.LAT_N >= s1.LAT_N
);

Report:

Note:


------

Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy: 


<img width="113" height="199" alt="image" src="https://github.com/user-attachments/assets/53aaea56-72c5-4369-9883-eb0c41ef60d6" />


Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.

Note:

The tables may contain duplicate records.
The company_code is string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

Input Format

The following tables contain company data:

Company: The company_code is the code of the company and founder is the founder of the company.

<img width="534" height="320" alt="Screenshot 2026-05-31 at 21 16 29" src="https://github.com/user-attachments/assets/74b4ad0d-3c30-4676-9165-e4703709ee3a" />


Lead_Manager: The lead_manager_code is the code of the lead manager, and the company_code is the code of the working company.

<img width="537" height="287" alt="Screenshot 2026-05-31 at 21 16 53" src="https://github.com/user-attachments/assets/2a86b70e-6c6c-4bda-8127-d8dad18b563a" />

Senior_Manager: The senior_manager_code is the code of the senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

<img width="553" height="345" alt="Screenshot 2026-05-31 at 21 17 13" src="https://github.com/user-attachments/assets/babf727d-5471-4083-b71c-573d4e8e5f06" />

Manager: The manager_code is the code of the manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

<img width="538" height="428" alt="Screenshot 2026-05-31 at 21 17 41" src="https://github.com/user-attachments/assets/7bbea55d-1612-431a-8680-18476b4151f3" />

Employee: The employee_code is the code of the employee, the manager_code is the code of its manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

<img width="550" height="500" alt="Screenshot 2026-05-31 at 21 18 05" src="https://github.com/user-attachments/assets/49a2b742-417d-4a5c-927a-1e7902f72cad" />

Answer:

SELECT 
    company_code, 
    founder, 
    (SELECT COUNT(DISTINCT lead_manager_code) FROM Lead_Manager WHERE company_code = c.company_code),
    (SELECT COUNT(DISTINCT senior_manager_code) FROM Senior_Manager WHERE company_code = c.company_code),
    (SELECT COUNT(DISTINCT manager_code) FROM Manager WHERE company_code = c.company_code),
    (SELECT COUNT(DISTINCT employee_code) FROM Employee WHERE company_code = c.company_code)
FROM Company c
ORDER BY company_code ASC;

Report:

Notes:

------
