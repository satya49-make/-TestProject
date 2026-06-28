# 📘 Core SQL Query-Based Interview Questions

## 1. Find the Second Highest Salary

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

---

## 2. Find Duplicate Rows in a Table

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 1;
```

---

## 3. Delete Duplicate Rows but Keep One

```sql
DELETE t1
FROM employees t1
JOIN employees t2
ON t1.id > t2.id
AND t1.email = t2.email;
```

---

## 4. Find Nth Highest Salary

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET n - 1;
```

---

## 5. Find Employees Who Don't Have Managers

```sql
SELECT e.name
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id
WHERE m.id IS NULL;
```

---

## 6. Find Department-wise Maximum Salary

```sql
SELECT department_id, MAX(salary)
FROM employees
GROUP BY department_id;
```

---

## 7. Find Employees with the Same Salary

```sql
SELECT salary, GROUP_CONCAT(name)
FROM employees
GROUP BY salary
HAVING COUNT(*) > 1;
```

---

## 8. Find Top 3 Salaries Per Department

```sql
SELECT department_id, salary
FROM (
    SELECT department_id,
           salary,
           RANK() OVER (
               PARTITION BY department_id
               ORDER BY salary DESC
           ) AS rnk
    FROM employees
) t
WHERE rnk <= 3;
```

---

## 9. Find Employees Hired in the Last 30 Days

```sql
SELECT *
FROM employees
WHERE hire_date >= CURDATE() - INTERVAL 30 DAY;
```

---

## 10. Find Employees with No Assigned Projects

```sql
SELECT e.name
FROM employees e
LEFT JOIN projects p
ON e.id = p.emp_id
WHERE p.emp_id IS NULL;
```

---

## 11. Find Average Salary Per Department

```sql
SELECT department_id,
       AVG(salary)
FROM employees
GROUP BY department_id;
```

---

## 12. Find Employees with Salary Greater than Department Average

```sql
SELECT e.name,
       e.salary
FROM employees e
JOIN (
    SELECT department_id,
           AVG(salary) AS avg_sal
    FROM employees
    GROUP BY department_id
) d
ON e.department_id = d.department_id
WHERE e.salary > d.avg_sal;
```

---

## 13. Find Employees Who Joined Before Their Managers

```sql
SELECT e.name,
       e.hire_date,
       m.name,
       m.hire_date
FROM employees e
JOIN employees m
ON e.manager_id = m.id
WHERE e.hire_date < m.hire_date;
```

---

## 14. Find Employees with the Highest Salary in Each Department

```sql
SELECT e.*
FROM employees e
JOIN (
    SELECT department_id,
           MAX(salary) AS max_sal
    FROM employees
    GROUP BY department_id
) d
ON e.department_id = d.department_id
AND e.salary = d.max_sal;
```

---

## 15. Pagination Query (Fetch Records 11–20)

```sql
SELECT *
FROM employees
ORDER BY id
LIMIT 10 OFFSET 10;
```

---

# 📘 Additional 20 Important Queries (Joins + Subqueries)

## 16. Find Employees Working on More Than One Project

```sql
SELECT emp_id,
       COUNT(*)
FROM project_assignments
GROUP BY emp_id
HAVING COUNT(*) > 1;
```

---

## 17. Find Employees Who Share the Same Manager

```sql
SELECT manager_id,
       GROUP_CONCAT(name)
FROM employees
GROUP BY manager_id
HAVING COUNT(*) > 1;
```

---

## 18. Find Employees Who Earn More Than Their Manager

```sql
SELECT e.name,
       e.salary,
       m.name AS manager,
       m.salary AS manager_salary
FROM employees e
JOIN employees m
ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

---

## 19. Find Employees Not Assigned to Any Department

```sql
SELECT e.name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id
WHERE d.id IS NULL;
```

---

## 20. Find Employees Who Joined in the Same Year as Their Manager

```sql
SELECT e.name,
       m.name AS manager
FROM employees e
JOIN employees m
ON e.manager_id = m.id
WHERE YEAR(e.hire_date) = YEAR(m.hire_date);
```

---

## 21. Find Employees Who Work in All Projects

```sql
SELECT emp_id
FROM project_assignments
GROUP BY emp_id
HAVING COUNT(DISTINCT project_id) =
(
    SELECT COUNT(*)
    FROM projects
);
```

---

## 22. Find Employees with Salaries in the Top 10%

```sql
SELECT *
FROM employees
WHERE salary >=
(
    SELECT PERCENTILE_CONT(0.9)
    WITHIN GROUP (ORDER BY salary)
    FROM employees
);
```

---

## 23. Find Employees with the Same Hire Date

```sql
SELECT hire_date,
       GROUP_CONCAT(name)
FROM employees
GROUP BY hire_date
HAVING COUNT(*) > 1;
```

---

## 24. Find Employees Not in the Project Table (Anti Join)

```sql
SELECT e.*
FROM employees e
WHERE NOT EXISTS
(
    SELECT 1
    FROM projects p
    WHERE p.emp_id = e.id
);
```

---

## 25. Find Employees with Maximum Salary

```sql
SELECT *
FROM employees
WHERE salary =
(
    SELECT MAX(salary)
    FROM employees
);
```

---

## 26. Employees Earning More Than Department Average

```sql
SELECT e.*
FROM employees e
WHERE e.salary >
(
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id
);
```

---

## 27. Employees with the Earliest Hire Date

```sql
SELECT *
FROM employees
WHERE hire_date =
(
    SELECT MIN(hire_date)
    FROM employees
);
```

---

## 28. Employees Assigned to More Than Three Projects

```sql
SELECT emp_id,
       COUNT(*)
FROM project_assignments
GROUP BY emp_id
HAVING COUNT(*) > 3;
```

---

## 29. Managers Not Assigned to Any Project

```sql
SELECT e.*
FROM employees e
WHERE e.id IN
(
    SELECT DISTINCT manager_id
    FROM employees
)
AND NOT EXISTS
(
    SELECT 1
    FROM projects p
    WHERE p.emp_id = e.id
);
```

---

## 30. Employees with Same Salary in Different Departments

```sql
SELECT DISTINCT
       e1.name,
       e1.salary,
       e1.department_id
FROM employees e1
JOIN employees e2
ON e1.salary = e2.salary
AND e1.department_id <> e2.department_id;
```

---

## 31. Employees Joined After Department Creation

```sql
SELECT e.name,
       e.hire_date,
       d.created_date
FROM employees e
JOIN departments d
ON e.department_id = d.id
WHERE e.hire_date > d.created_date;
```

---

## 32. Employees Assigned to All Projects in Their Department

```sql
SELECT e.id,
       e.name
FROM employees e
WHERE NOT EXISTS
(
    SELECT p.project_id
    FROM projects p
    WHERE p.department_id = e.department_id
    AND NOT EXISTS
    (
        SELECT 1
        FROM project_assignments pa
        WHERE pa.emp_id = e.id
        AND pa.project_id = p.project_id
    )
);
```

---

## 33. Employees Earning More Than Company Average

```sql
SELECT *
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

---

## 34. Find Employees Who Are Not Managers

```sql
SELECT *
FROM employees
WHERE id NOT IN
(
    SELECT DISTINCT manager_id
    FROM employees
    WHERE manager_id IS NOT NULL
);
```

---

## 35. Employees Joined Last Year and Earn Above Average Salary

```sql
SELECT *
FROM employees
WHERE hire_date >= CURDATE() - INTERVAL 1 YEAR
AND salary >
(
    SELECT AVG(salary)
    FROM employees
);
```

---