---
layout: default
title: SQL Complete Reference
---

# SQL Complete Reference Guide

A comprehensive reference covering SQL functions, clauses, and patterns — from fundamentals to advanced techniques. Each section explains the concept, syntax, when to use it, and practical examples.

All examples use these sample tables:

```sql
-- employees
CREATE TABLE employees (
    id         INT PRIMARY KEY,
    name       VARCHAR(100),
    department VARCHAR(50),
    salary     DECIMAL(10,2),
    hire_date  DATE,
    manager_id INT
);

-- orders
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    customer_id INT,
    product     VARCHAR(100),
    quantity    INT,
    price       DECIMAL(10,2),
    order_date  DATE,
    status      VARCHAR(20)
);

-- customers
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name        VARCHAR(100),
    city        VARCHAR(50),
    country     VARCHAR(50),
    email       VARCHAR(100),
    created_at  DATE
);
```

---

## Table of Contents

1. [SELECT and Filtering](#1-select-and-filtering)
2. [WHERE Clause and Operators](#2-where-clause-and-operators)
3. [Wildcards and Pattern Matching](#3-wildcards-and-pattern-matching)
4. [ORDER BY](#4-order-by)
5. [Aggregate Functions](#5-aggregate-functions)
6. [GROUP BY](#6-group-by)
7. [HAVING](#7-having)
8. [CASE Expressions](#8-case-expressions)
9. [JOINS](#9-joins)
10. [Subqueries](#10-subqueries)
11. [Common Table Expressions (CTEs)](#11-common-table-expressions-ctes)
12. [Window Functions and PARTITION BY](#12-window-functions-and-partition-by)
13. [Set Operations](#13-set-operations)
14. [String Functions](#14-string-functions)
15. [Date and Time Functions](#15-date-and-time-functions)
16. [Numeric Functions](#16-numeric-functions)
17. [NULL Handling](#17-null-handling)
18. [EXISTS and IN](#18-exists-and-in)
19. [INSERT, UPDATE, DELETE](#19-insert-update-delete)
20. [MERGE / UPSERT](#20-merge--upsert)
21. [Views](#21-views)
22. [Indexes](#22-indexes)
23. [Transactions](#23-transactions)
24. [Constraints](#24-constraints)
25. [Temporary Tables and Table Variables](#25-temporary-tables-and-table-variables)
26. [Pivoting and Unpivoting](#26-pivoting-and-unpivoting)
27. [Recursive CTEs](#27-recursive-ctes)
28. [Query Execution Order](#28-query-execution-order)
29. [Data Types Reference](#29-data-types-reference)
30. [ALTER TABLE — Schema Changes](#30-alter-table--schema-changes)
31. [GROUPING SETS, ROLLUP, and CUBE](#31-grouping-sets-rollup-and-cube)
32. [LATERAL Joins](#32-lateral-joins)
33. [ANY, ALL, and SOME Operators](#33-any-all-and-some-operators)
34. [JSON Functions](#34-json-functions)
35. [Statistical and Advanced Window Functions](#35-statistical-and-advanced-window-functions)
36. [EXPLAIN — Reading Query Plans](#36-explain--reading-query-plans)
37. [Performance Patterns and Anti-Patterns](#37-performance-patterns-and-anti-patterns)

---

## 1. SELECT and Filtering

### Basic SELECT

Retrieves columns from a table.

```sql
-- select specific columns
SELECT name, department, salary
FROM employees;

-- select all columns
SELECT *
FROM employees;

-- select with alias
SELECT name AS employee_name, salary AS annual_pay
FROM employees;

-- select distinct values (removes duplicates)
SELECT DISTINCT department
FROM employees;

-- select with computed column
SELECT name, salary, salary * 12 AS annual_salary
FROM employees;
```

### LIMIT / TOP

Restrict the number of rows returned.

```sql
-- MySQL / PostgreSQL / SQLite
SELECT * FROM employees LIMIT 10;

-- with offset (skip first 5, return next 10)
SELECT * FROM employees LIMIT 10 OFFSET 5;

-- SQL Server
SELECT TOP 10 * FROM employees;

-- SQL Server with OFFSET-FETCH (requires ORDER BY)
SELECT * FROM employees
ORDER BY salary DESC
OFFSET 5 ROWS FETCH NEXT 10 ROWS ONLY;
```

**When to use:** Pagination, previewing data, returning top-N results.

---

## 2. WHERE Clause and Operators

Filters rows before any grouping happens.

### Comparison Operators

```sql
-- equals
SELECT * FROM employees WHERE department = 'Engineering';

-- not equals
SELECT * FROM employees WHERE department != 'Sales';
SELECT * FROM employees WHERE department <> 'Sales';  -- same thing

-- greater than, less than
SELECT * FROM employees WHERE salary > 70000;
SELECT * FROM employees WHERE salary <= 50000;
```

### Logical Operators

```sql
-- AND: both conditions must be true
SELECT * FROM employees
WHERE department = 'Engineering' AND salary > 80000;

-- OR: at least one condition must be true
SELECT * FROM employees
WHERE department = 'Engineering' OR department = 'Sales';

-- NOT: negates a condition
SELECT * FROM employees
WHERE NOT department = 'HR';
```

### BETWEEN

```sql
-- inclusive range
SELECT * FROM employees
WHERE salary BETWEEN 50000 AND 80000;

-- date range
SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

### IN

```sql
-- match any value in a list
SELECT * FROM employees
WHERE department IN ('Engineering', 'Sales', 'Marketing');

-- NOT IN
SELECT * FROM employees
WHERE department NOT IN ('HR', 'Finance');
```

### IS NULL / IS NOT NULL

```sql
SELECT * FROM employees WHERE manager_id IS NULL;      -- top-level managers
SELECT * FROM employees WHERE manager_id IS NOT NULL;   -- has a manager
```

**When to use:** Always use WHERE to filter rows before aggregation. Use HAVING (section 7) to filter after aggregation.

---

## 3. Wildcards and Pattern Matching

### LIKE

Pattern matching on string columns. Two wildcard characters:
- `%` matches zero or more characters
- `_` matches exactly one character

```sql
-- starts with 'A'
SELECT * FROM customers WHERE name LIKE 'A%';

-- ends with 'son'
SELECT * FROM customers WHERE name LIKE '%son';

-- contains 'mar'
SELECT * FROM customers WHERE name LIKE '%mar%';

-- second character is 'a'
SELECT * FROM customers WHERE name LIKE '_a%';

-- exactly 5 characters
SELECT * FROM customers WHERE name LIKE '_____';

-- NOT LIKE
SELECT * FROM customers WHERE email NOT LIKE '%@gmail.com';
```

### Case-Insensitive Matching

```sql
-- PostgreSQL: ILIKE
SELECT * FROM customers WHERE name ILIKE '%john%';

-- MySQL: LIKE is case-insensitive by default (depends on collation)

-- SQL Server / general approach
SELECT * FROM customers WHERE LOWER(name) LIKE '%john%';
```

### SIMILAR TO / REGEXP (PostgreSQL / MySQL)

```sql
-- PostgreSQL: regex match
SELECT * FROM customers WHERE email ~ '^[a-z]+@gmail\.com$';

-- MySQL: REGEXP
SELECT * FROM customers WHERE email REGEXP '^[a-z]+@gmail\\.com$';
```

**When to use:** Searching text fields by pattern — name lookups, email domain filtering, partial matches.

---

## 4. ORDER BY

Sorts the result set.

```sql
-- ascending (default)
SELECT * FROM employees ORDER BY salary;
SELECT * FROM employees ORDER BY salary ASC;

-- descending
SELECT * FROM employees ORDER BY salary DESC;

-- multiple columns
SELECT * FROM employees
ORDER BY department ASC, salary DESC;

-- order by column position (not recommended for readability)
SELECT name, department, salary FROM employees ORDER BY 3 DESC;

-- order by expression
SELECT name, salary FROM employees ORDER BY salary * 12 DESC;

-- order with NULLs control (PostgreSQL)
SELECT * FROM employees ORDER BY manager_id NULLS LAST;
SELECT * FROM employees ORDER BY manager_id NULLS FIRST;
```

**When to use:** Presenting results in a meaningful order. Required for TOP/LIMIT queries to be deterministic, and for window functions.

---

## 5. Aggregate Functions

Compute a single value from a set of rows.

| Function | Description |
|----------|-------------|
| `COUNT(*)` | Number of rows |
| `COUNT(column)` | Number of non-NULL values |
| `COUNT(DISTINCT col)` | Number of unique non-NULL values |
| `SUM(column)` | Total of numeric values |
| `AVG(column)` | Average of numeric values |
| `MIN(column)` | Smallest value |
| `MAX(column)` | Largest value |

```sql
-- total employees
SELECT COUNT(*) AS total_employees FROM employees;

-- count unique departments
SELECT COUNT(DISTINCT department) AS dept_count FROM employees;

-- salary statistics
SELECT
    MIN(salary)  AS lowest_salary,
    MAX(salary)  AS highest_salary,
    AVG(salary)  AS average_salary,
    SUM(salary)  AS total_payroll,
    COUNT(*)     AS headcount
FROM employees;

-- count vs count distinct
SELECT
    COUNT(department)          AS total_rows,        -- counts all non-null
    COUNT(DISTINCT department) AS unique_departments  -- counts unique non-null
FROM employees;
```

**When to use:** Summarizing data — totals, averages, counts. Almost always paired with GROUP BY for per-group summaries.

---

## 6. GROUP BY

Groups rows that share column values, so aggregate functions compute per-group results.

```sql
-- headcount per department
SELECT department, COUNT(*) AS headcount
FROM employees
GROUP BY department;

-- average salary per department
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;

-- total revenue per product
SELECT product, SUM(quantity * price) AS total_revenue
FROM orders
GROUP BY product;

-- group by multiple columns
SELECT department, status, COUNT(*) AS count
FROM employees
GROUP BY department, status;

-- group by with ORDER BY
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;
```

### Rule: Every non-aggregated column in SELECT must appear in GROUP BY

```sql
-- WRONG: 'name' is not aggregated and not in GROUP BY
SELECT name, department, AVG(salary)
FROM employees
GROUP BY department;

-- CORRECT
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

**When to use:** Whenever you need per-group summaries — "per department," "per month," "per customer."

---

## 7. HAVING

Filters groups after aggregation. Works like WHERE but for aggregated results.

### WHERE vs HAVING

| Clause | Filters | Timing |
|--------|---------|--------|
| WHERE | Individual rows | Before grouping |
| HAVING | Grouped results | After grouping |

```sql
-- departments with average salary above 70,000
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 70000;

-- departments with more than 5 employees
SELECT department, COUNT(*) AS headcount
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;

-- combining WHERE and HAVING
-- "Among employees hired after 2020, find departments with avg salary > 60k"
SELECT department, AVG(salary) AS avg_salary
FROM employees
WHERE hire_date > '2020-01-01'       -- filters rows BEFORE grouping
GROUP BY department
HAVING AVG(salary) > 60000;          -- filters groups AFTER aggregation

-- customers who placed more than 3 orders totaling over $1000
SELECT customer_id, COUNT(*) AS order_count, SUM(price * quantity) AS total_spent
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 3 AND SUM(price * quantity) > 1000;
```

**When to use:** When your filter condition involves an aggregate function (COUNT, SUM, AVG, etc.). If you can filter with WHERE, prefer it — it's more efficient because it reduces rows before grouping.

---

## 8. CASE Expressions

SQL's if-then-else. Returns a value based on conditions.

### Simple CASE

```sql
SELECT name, department,
    CASE department
        WHEN 'Engineering' THEN 'Tech'
        WHEN 'Sales'       THEN 'Revenue'
        WHEN 'HR'          THEN 'People'
        ELSE 'Other'
    END AS dept_category
FROM employees;
```

### Searched CASE (more flexible)

```sql
-- salary tier classification
SELECT name, salary,
    CASE
        WHEN salary >= 100000 THEN 'Senior'
        WHEN salary >= 70000  THEN 'Mid-Level'
        WHEN salary >= 40000  THEN 'Junior'
        ELSE 'Entry'
    END AS salary_tier
FROM employees;
```

### CASE in Aggregations

```sql
-- count employees per salary tier per department
SELECT department,
    COUNT(CASE WHEN salary >= 100000 THEN 1 END) AS senior_count,
    COUNT(CASE WHEN salary >= 70000 AND salary < 100000 THEN 1 END) AS mid_count,
    COUNT(CASE WHEN salary < 70000 THEN 1 END) AS junior_count
FROM employees
GROUP BY department;

-- conditional sum
SELECT
    SUM(CASE WHEN status = 'completed' THEN price * quantity ELSE 0 END) AS completed_revenue,
    SUM(CASE WHEN status = 'pending'   THEN price * quantity ELSE 0 END) AS pending_revenue
FROM orders;
```

### CASE in ORDER BY

```sql
-- custom sort order
SELECT * FROM employees
ORDER BY
    CASE department
        WHEN 'Engineering' THEN 1
        WHEN 'Sales'       THEN 2
        WHEN 'HR'          THEN 3
        ELSE 4
    END;
```

### CASE in WHERE

```sql
SELECT * FROM orders
WHERE
    CASE
        WHEN status = 'completed' THEN price * quantity > 100
        WHEN status = 'pending'   THEN price * quantity > 50
        ELSE TRUE
    END;
```

**When to use:** Conditional logic inside queries — bucketing, conditional counting, pivoting, custom sorting, mapping codes to labels.

---

## 9. JOINS

Combine rows from two or more tables based on a related column.

### INNER JOIN

Returns only rows that have a match in both tables.

```sql
SELECT o.order_id, c.name, o.product, o.price
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;
```

### LEFT JOIN (LEFT OUTER JOIN)

Returns all rows from the left table, plus matching rows from the right. Non-matching right-side columns are NULL.

```sql
-- all customers, even those with no orders
SELECT c.name, o.order_id, o.product
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;

-- find customers who never ordered
SELECT c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### RIGHT JOIN (RIGHT OUTER JOIN)

Returns all rows from the right table. Less common — you can always rewrite as a LEFT JOIN by swapping table positions.

```sql
SELECT o.order_id, c.name
FROM orders o
RIGHT JOIN customers c ON o.customer_id = c.customer_id;
```

### FULL OUTER JOIN

Returns all rows from both tables. NULLs fill in where there's no match.

```sql
SELECT c.name, o.order_id
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```

### CROSS JOIN

Returns the Cartesian product — every row from table A paired with every row from table B.

```sql
-- every employee paired with every department
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;

-- practical use: generate a calendar grid
SELECT months.m, years.y
FROM (SELECT 1 AS m UNION SELECT 2 UNION SELECT 3) months
CROSS JOIN (SELECT 2023 AS y UNION SELECT 2024) years;
```

### SELF JOIN

A table joined to itself. Uses aliases to distinguish the two "copies."

```sql
-- find each employee's manager name
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;

-- find employees who earn more than their manager
SELECT e.name AS employee, e.salary AS emp_salary,
       m.name AS manager, m.salary AS mgr_salary
FROM employees e
INNER JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

### Joining Multiple Tables

```sql
SELECT c.name AS customer, o.order_id, o.product, p.category
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
INNER JOIN products p ON o.product = p.product_name;
```

### Join Conditions Beyond Equality

```sql
-- range join: find the tax bracket for each employee's salary
SELECT e.name, e.salary, t.bracket, t.rate
FROM employees e
INNER JOIN tax_brackets t
    ON e.salary >= t.min_salary AND e.salary < t.max_salary;
```

### When to Use Each Join

| Join Type | Use When |
|-----------|----------|
| INNER JOIN | You only want rows that match in both tables |
| LEFT JOIN | You want all rows from the left table, even without matches |
| RIGHT JOIN | You want all rows from the right table (prefer LEFT JOIN instead) |
| FULL OUTER JOIN | You want all rows from both tables, matched or not |
| CROSS JOIN | You need every combination (Cartesian product) |
| SELF JOIN | Comparing rows within the same table (hierarchies, pairs) |

---

## 10. Subqueries

A query nested inside another query.

### Subquery in WHERE

```sql
-- employees earning above the company average
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- employees in departments that have more than 10 people
SELECT name, department
FROM employees
WHERE department IN (
    SELECT department
    FROM employees
    GROUP BY department
    HAVING COUNT(*) > 10
);
```

### Correlated Subquery

References the outer query. Executes once per outer row.

```sql
-- employees who earn more than their department's average
SELECT e.name, e.salary, e.department
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department = e.department    -- references outer query
);

-- most recent order per customer
SELECT o.*
FROM orders o
WHERE o.order_date = (
    SELECT MAX(o2.order_date)
    FROM orders o2
    WHERE o2.customer_id = o.customer_id
);
```

### Subquery in SELECT (Scalar Subquery)

Must return exactly one value.

```sql
SELECT name, salary,
    (SELECT AVG(salary) FROM employees) AS company_avg,
    salary - (SELECT AVG(salary) FROM employees) AS diff_from_avg
FROM employees;
```

### Subquery in FROM (Derived Table)

```sql
-- average of department averages
SELECT AVG(dept_avg) AS avg_of_avgs
FROM (
    SELECT department, AVG(salary) AS dept_avg
    FROM employees
    GROUP BY department
) dept_summary;
```

### Subquery in JOIN

```sql
SELECT e.name, e.department, d.avg_salary
FROM employees e
INNER JOIN (
    SELECT department, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
) d ON e.department = d.department
WHERE e.salary > d.avg_salary;
```

**When to use:** When you need results from one query to drive another. Prefer CTEs (section 11) for readability when subqueries get deeply nested.

---

## 11. Common Table Expressions (CTEs)

Named temporary result sets that make complex queries readable. Defined with `WITH`.

### Basic CTE

```sql
WITH dept_stats AS (
    SELECT department,
           AVG(salary) AS avg_salary,
           COUNT(*)    AS headcount
    FROM employees
    GROUP BY department
)
SELECT e.name, e.salary, d.avg_salary, d.headcount
FROM employees e
INNER JOIN dept_stats d ON e.department = d.department
WHERE e.salary > d.avg_salary;
```

### Multiple CTEs

```sql
WITH high_value_customers AS (
    SELECT customer_id, SUM(price * quantity) AS total_spent
    FROM orders
    GROUP BY customer_id
    HAVING SUM(price * quantity) > 5000
),
recent_orders AS (
    SELECT customer_id, MAX(order_date) AS last_order
    FROM orders
    GROUP BY customer_id
)
SELECT c.name, h.total_spent, r.last_order
FROM customers c
INNER JOIN high_value_customers h ON c.customer_id = h.customer_id
INNER JOIN recent_orders r ON c.customer_id = r.customer_id;
```

### CTE vs Subquery

| CTE | Subquery |
|-----|----------|
| Named, readable | Anonymous, inline |
| Can reference earlier CTEs | Can't reference sibling subqueries |
| Can be recursive | Can't be recursive |
| Defined once, used multiple times | Repeated if used in multiple places |

**When to use:** Anytime a subquery would make the code hard to read. Multi-step transformations, reusing the same derived table multiple times, recursive queries.

---

## 12. Window Functions and PARTITION BY

Perform calculations across a set of rows related to the current row — without collapsing rows like GROUP BY does.

### Syntax

```sql
function_name(...) OVER (
    [PARTITION BY column(s)]
    [ORDER BY column(s)]
    [frame_clause]
)
```

### ROW_NUMBER

Assigns a unique sequential number within each partition.

```sql
-- number employees within each department by salary (highest first)
SELECT name, department, salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_in_dept
FROM employees;

-- get the top earner per department
WITH ranked AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rn
    FROM employees
)
SELECT * FROM ranked WHERE rn = 1;
```

### RANK and DENSE_RANK

```sql
SELECT name, department, salary,
    RANK()       OVER (ORDER BY salary DESC) AS rank,        -- gaps after ties
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank   -- no gaps
FROM employees;

-- Example output:
-- salary=100k → rank=1, dense_rank=1
-- salary=100k → rank=1, dense_rank=1
-- salary=90k  → rank=3, dense_rank=2  ← RANK skips 2; DENSE_RANK doesn't
```

### NTILE

Divides rows into N roughly equal buckets.

```sql
-- divide employees into 4 salary quartiles
SELECT name, salary,
    NTILE(4) OVER (ORDER BY salary) AS salary_quartile
FROM employees;
```

### Aggregate Window Functions

Run aggregates without collapsing rows.

```sql
SELECT name, department, salary,
    AVG(salary) OVER (PARTITION BY department) AS dept_avg,
    SUM(salary) OVER (PARTITION BY department) AS dept_total,
    COUNT(*)    OVER (PARTITION BY department) AS dept_count,
    salary - AVG(salary) OVER (PARTITION BY department) AS diff_from_dept_avg
FROM employees;
```

### LAG and LEAD

Access previous or next row's value.

```sql
-- compare each order's revenue to the previous order
SELECT order_id, order_date, price * quantity AS revenue,
    LAG(price * quantity, 1)  OVER (ORDER BY order_date) AS prev_revenue,
    LEAD(price * quantity, 1) OVER (ORDER BY order_date) AS next_revenue
FROM orders;

-- month-over-month change
WITH monthly AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(price * quantity) AS revenue
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
)
SELECT month, revenue,
    LAG(revenue) OVER (ORDER BY month) AS prev_month,
    revenue - LAG(revenue) OVER (ORDER BY month) AS change,
    ROUND(
        (revenue - LAG(revenue) OVER (ORDER BY month))
        / LAG(revenue) OVER (ORDER BY month) * 100, 2
    ) AS pct_change
FROM monthly;
```

### FIRST_VALUE and LAST_VALUE

```sql
SELECT name, department, salary,
    FIRST_VALUE(name) OVER (
        PARTITION BY department ORDER BY salary DESC
    ) AS highest_paid_in_dept
FROM employees;
```

### Running Totals (Window Frame)

```sql
-- cumulative salary sum within department
SELECT name, department, salary,
    SUM(salary) OVER (
        PARTITION BY department
        ORDER BY hire_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM employees;

-- 3-row moving average
SELECT order_date, price * quantity AS revenue,
    AVG(price * quantity) OVER (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3
FROM orders;
```

### PARTITION BY vs GROUP BY

| PARTITION BY | GROUP BY |
|-------------|----------|
| Keeps all rows | Collapses to one row per group |
| Used with window functions | Used with aggregate functions |
| Can appear alongside non-aggregated columns | Non-aggregated columns must be in GROUP BY |

**When to use:** Rankings, running totals, row comparisons (lag/lead), per-group calculations without losing row detail.

---

## 13. Set Operations

Combine results from two or more SELECT statements.

### UNION / UNION ALL

```sql
-- UNION: combines and removes duplicates
SELECT name, city FROM customers
UNION
SELECT name, city FROM suppliers;

-- UNION ALL: combines and keeps duplicates (faster)
SELECT name, city FROM customers
UNION ALL
SELECT name, city FROM suppliers;
```

### INTERSECT

Returns rows that appear in both queries.

```sql
SELECT customer_id FROM orders WHERE product = 'Laptop'
INTERSECT
SELECT customer_id FROM orders WHERE product = 'Mouse';
-- customers who bought BOTH a laptop and a mouse
```

### EXCEPT / MINUS

Returns rows from the first query that don't appear in the second.

```sql
SELECT customer_id FROM customers
EXCEPT
SELECT DISTINCT customer_id FROM orders;
-- customers who have never placed an order
```

### Rules

- All queries must have the same number of columns
- Corresponding columns must have compatible data types
- Column names come from the first query

**When to use:** Combining similar result sets, finding overlaps or differences between data sets.

---

## 14. String Functions

| Function | Description | Example | Result |
|----------|-------------|---------|--------|
| `UPPER(s)` | Uppercase | `UPPER('hello')` | `HELLO` |
| `LOWER(s)` | Lowercase | `LOWER('HELLO')` | `hello` |
| `LENGTH(s)` / `LEN(s)` | String length | `LENGTH('hello')` | `5` |
| `TRIM(s)` | Remove leading/trailing spaces | `TRIM('  hi  ')` | `hi` |
| `LTRIM(s)` / `RTRIM(s)` | Left/right trim | `LTRIM('  hi')` | `hi` |
| `SUBSTRING(s, start, len)` | Extract part | `SUBSTRING('hello', 2, 3)` | `ell` |
| `LEFT(s, n)` / `RIGHT(s, n)` | First/last n chars | `LEFT('hello', 3)` | `hel` |
| `REPLACE(s, old, new)` | Replace occurrences | `REPLACE('hello', 'l', 'r')` | `herro` |
| `CONCAT(s1, s2, ...)` | Concatenate | `CONCAT('hello', ' ', 'world')` | `hello world` |
| `CHARINDEX(sub, s)` | Find position (SQL Server) | `CHARINDEX('ll', 'hello')` | `3` |
| `POSITION(sub IN s)` | Find position (PostgreSQL) | `POSITION('ll' IN 'hello')` | `3` |
| `REVERSE(s)` | Reverse string | `REVERSE('hello')` | `olleh` |
| `REPEAT(s, n)` | Repeat string | `REPEAT('ab', 3)` | `ababab` |
| `LPAD(s, len, pad)` | Left-pad | `LPAD('42', 5, '0')` | `00042` |
| `RPAD(s, len, pad)` | Right-pad | `RPAD('hi', 5, '.')` | `hi...` |

```sql
-- practical: extract email domain
SELECT email,
    SUBSTRING(email, POSITION('@' IN email) + 1, LENGTH(email)) AS domain
FROM customers;

-- practical: clean and standardize names
SELECT CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2, LENGTH(name)))) AS proper_name
FROM customers;

-- practical: mask email
SELECT CONCAT(LEFT(email, 2), '****@', SUBSTRING(email, POSITION('@' IN email) + 1, LENGTH(email))) AS masked
FROM customers;
```

### CONCAT_WS (Concatenate With Separator)

```sql
SELECT CONCAT_WS(', ', city, country) AS location
FROM customers;
-- Output: "New York, USA"
```

### STRING_AGG / GROUP_CONCAT

Aggregate strings into a single delimited value.

```sql
-- PostgreSQL
SELECT department, STRING_AGG(name, ', ' ORDER BY name) AS team_members
FROM employees
GROUP BY department;

-- MySQL
SELECT department, GROUP_CONCAT(name ORDER BY name SEPARATOR ', ') AS team_members
FROM employees
GROUP BY department;
```

---

## 15. Date and Time Functions

### Getting Current Date/Time

```sql
-- PostgreSQL
SELECT CURRENT_DATE, CURRENT_TIMESTAMP, NOW();

-- MySQL
SELECT CURDATE(), NOW(), CURRENT_TIMESTAMP();

-- SQL Server
SELECT GETDATE(), SYSDATETIME(), CAST(GETDATE() AS DATE);
```

### Extracting Parts

```sql
-- EXTRACT (PostgreSQL, MySQL)
SELECT EXTRACT(YEAR FROM order_date)  AS year,
       EXTRACT(MONTH FROM order_date) AS month,
       EXTRACT(DAY FROM order_date)   AS day,
       EXTRACT(DOW FROM order_date)   AS day_of_week
FROM orders;

-- DATEPART (SQL Server)
SELECT DATEPART(YEAR, order_date)    AS year,
       DATEPART(MONTH, order_date)   AS month,
       DATEPART(WEEKDAY, order_date) AS weekday
FROM orders;

-- YEAR(), MONTH(), DAY() (MySQL, SQL Server)
SELECT YEAR(order_date), MONTH(order_date), DAY(order_date)
FROM orders;
```

### Date Arithmetic

```sql
-- PostgreSQL: interval arithmetic
SELECT order_date + INTERVAL '30 days' AS due_date FROM orders;
SELECT NOW() - INTERVAL '7 days' AS one_week_ago;

-- MySQL
SELECT DATE_ADD(order_date, INTERVAL 30 DAY) AS due_date FROM orders;
SELECT DATE_SUB(NOW(), INTERVAL 7 DAY) AS one_week_ago;

-- SQL Server
SELECT DATEADD(DAY, 30, order_date) AS due_date FROM orders;
SELECT DATEDIFF(DAY, hire_date, GETDATE()) AS days_employed FROM employees;
```

### DATE_TRUNC (PostgreSQL) / Rounding Dates

```sql
-- truncate to month
SELECT DATE_TRUNC('month', order_date) AS order_month, SUM(price * quantity) AS revenue
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY order_month;

-- truncate to quarter
SELECT DATE_TRUNC('quarter', order_date) AS quarter, SUM(price * quantity)
FROM orders
GROUP BY DATE_TRUNC('quarter', order_date);
```

### Formatting Dates

```sql
-- PostgreSQL
SELECT TO_CHAR(order_date, 'YYYY-MM-DD') FROM orders;
SELECT TO_CHAR(order_date, 'Month DD, YYYY') FROM orders;

-- MySQL
SELECT DATE_FORMAT(order_date, '%Y-%m-%d') FROM orders;
SELECT DATE_FORMAT(order_date, '%M %d, %Y') FROM orders;

-- SQL Server
SELECT FORMAT(order_date, 'yyyy-MM-dd') FROM orders;
SELECT CONVERT(VARCHAR, order_date, 101) FROM orders;  -- MM/DD/YYYY
```

**When to use:** Grouping by time period, calculating durations, date filtering, generating date-based reports.

---

## 16. Numeric Functions

| Function | Description | Example | Result |
|----------|-------------|---------|--------|
| `ROUND(n, d)` | Round to d decimals | `ROUND(3.14159, 2)` | `3.14` |
| `CEIL(n)` / `CEILING(n)` | Round up | `CEIL(3.2)` | `4` |
| `FLOOR(n)` | Round down | `FLOOR(3.8)` | `3` |
| `ABS(n)` | Absolute value | `ABS(-5)` | `5` |
| `MOD(n, d)` / `n % d` | Remainder | `MOD(10, 3)` | `1` |
| `POWER(n, p)` | Exponent | `POWER(2, 3)` | `8` |
| `SQRT(n)` | Square root | `SQRT(16)` | `4` |
| `SIGN(n)` | Sign (-1, 0, 1) | `SIGN(-5)` | `-1` |
| `GREATEST(a,b,...)` | Largest value | `GREATEST(1, 5, 3)` | `5` |
| `LEAST(a,b,...)` | Smallest value | `LEAST(1, 5, 3)` | `1` |

```sql
-- practical: round salary to nearest thousand
SELECT name, salary, ROUND(salary, -3) AS rounded_salary
FROM employees;

-- practical: calculate percentage
SELECT department,
    COUNT(*) AS dept_count,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM employees), 1) AS pct_of_total
FROM employees
GROUP BY department;
```

### CAST / CONVERT

```sql
-- type conversion
SELECT CAST(salary AS INTEGER) FROM employees;
SELECT CAST('2024-01-15' AS DATE);
SELECT CAST(price AS DECIMAL(10,2)) FROM orders;

-- SQL Server
SELECT CONVERT(INT, salary) FROM employees;
```

---

## 17. NULL Handling

### COALESCE

Returns the first non-NULL value in the list.

```sql
-- default value for NULL
SELECT name, COALESCE(department, 'Unassigned') AS department
FROM employees;

-- chain of fallbacks
SELECT COALESCE(phone, mobile, email, 'No Contact') AS contact
FROM customers;
```

### NULLIF

Returns NULL if two values are equal, otherwise returns the first value.

```sql
-- avoid division by zero
SELECT revenue / NULLIF(cost, 0) AS margin
FROM products;

-- treat empty strings as NULL
SELECT COALESCE(NULLIF(TRIM(notes), ''), 'No notes') AS notes
FROM orders;
```

### IFNULL / ISNULL

```sql
-- MySQL: IFNULL
SELECT IFNULL(department, 'Unassigned') FROM employees;

-- SQL Server: ISNULL
SELECT ISNULL(department, 'Unassigned') FROM employees;
```

### NULLs in Aggregation

```sql
-- COUNT(*) counts all rows; COUNT(column) skips NULLs
SELECT
    COUNT(*) AS total_rows,
    COUNT(manager_id) AS has_manager,
    COUNT(*) - COUNT(manager_id) AS no_manager
FROM employees;

-- AVG ignores NULLs — this can be surprising
-- If values are [100, NULL, 200], AVG = 150 (not 100)
```

**When to use:** Anytime you need to handle missing data — defaults, fallbacks, safe division.

---

## 18. EXISTS and IN

### EXISTS

Returns TRUE if the subquery returns at least one row. Efficient for checking existence.

```sql
-- customers who have placed at least one order
SELECT c.name
FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id
);

-- customers who have never ordered
SELECT c.name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id
);
```

### IN vs EXISTS

```sql
-- these are logically equivalent but may perform differently

-- IN: good when subquery result set is small
SELECT * FROM employees
WHERE department IN (SELECT department_name FROM departments WHERE active = 1);

-- EXISTS: good when outer table is small relative to subquery, or subquery is correlated
SELECT * FROM employees e
WHERE EXISTS (SELECT 1 FROM departments d WHERE d.department_name = e.department AND d.active = 1);
```

### Performance Guidance

| Scenario | Prefer |
|----------|--------|
| Small subquery result, large outer table | IN |
| Large subquery result, small outer table | EXISTS |
| Subquery references outer query | EXISTS (it's already correlated) |
| Checking for absence | NOT EXISTS (handles NULLs correctly) |

**Important:** `NOT IN` behaves unexpectedly with NULLs. If the subquery returns any NULL, `NOT IN` returns no rows at all. Use `NOT EXISTS` to be safe.

```sql
-- DANGEROUS: if any customer_id in orders is NULL, this returns nothing
SELECT * FROM customers
WHERE customer_id NOT IN (SELECT customer_id FROM orders);

-- SAFE: NOT EXISTS handles NULLs correctly
SELECT * FROM customers c
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

---

## 19. INSERT, UPDATE, DELETE

### INSERT

```sql
-- single row
INSERT INTO employees (id, name, department, salary, hire_date)
VALUES (101, 'Alice Smith', 'Engineering', 95000, '2024-03-15');

-- multiple rows
INSERT INTO employees (id, name, department, salary, hire_date) VALUES
    (102, 'Bob Jones',    'Sales',       65000, '2024-04-01'),
    (103, 'Carol Lee',    'Engineering', 88000, '2024-04-15'),
    (104, 'David Chen',   'HR',          72000, '2024-05-01');

-- insert from a query
INSERT INTO employee_archive (id, name, department, salary)
SELECT id, name, department, salary
FROM employees
WHERE hire_date < '2020-01-01';
```

### UPDATE

```sql
-- update specific rows
UPDATE employees
SET salary = salary * 1.10
WHERE department = 'Engineering';

-- update with a subquery
UPDATE employees
SET salary = salary * 1.05
WHERE department IN (
    SELECT department FROM employees
    GROUP BY department
    HAVING AVG(salary) < 60000
);

-- update from another table (PostgreSQL)
UPDATE orders o
SET status = 'VIP'
FROM customers c
WHERE o.customer_id = c.customer_id AND c.country = 'USA';

-- update from another table (SQL Server)
UPDATE o
SET o.status = 'VIP'
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
WHERE c.country = 'USA';
```

### DELETE

```sql
-- delete specific rows
DELETE FROM orders WHERE status = 'cancelled';

-- delete with subquery
DELETE FROM customers
WHERE customer_id NOT IN (SELECT DISTINCT customer_id FROM orders);

-- TRUNCATE: delete all rows (faster than DELETE, non-transactional in most DBs)
TRUNCATE TABLE staging_data;
```

---

## 20. MERGE / UPSERT

Insert if new, update if exists.

### SQL Server / Standard SQL: MERGE

```sql
MERGE INTO employees AS target
USING new_employee_data AS source
ON target.id = source.id
WHEN MATCHED THEN
    UPDATE SET
        target.name = source.name,
        target.salary = source.salary
WHEN NOT MATCHED THEN
    INSERT (id, name, department, salary, hire_date)
    VALUES (source.id, source.name, source.department, source.salary, source.hire_date)
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;
```

### PostgreSQL: INSERT ON CONFLICT

```sql
INSERT INTO employees (id, name, department, salary)
VALUES (101, 'Alice Smith', 'Engineering', 95000)
ON CONFLICT (id) DO UPDATE
SET name = EXCLUDED.name,
    salary = EXCLUDED.salary;

-- upsert: do nothing on conflict
INSERT INTO employees (id, name, department, salary)
VALUES (101, 'Alice Smith', 'Engineering', 95000)
ON CONFLICT (id) DO NOTHING;
```

### MySQL: INSERT ON DUPLICATE KEY

```sql
INSERT INTO employees (id, name, department, salary)
VALUES (101, 'Alice Smith', 'Engineering', 95000)
ON DUPLICATE KEY UPDATE
    name = VALUES(name),
    salary = VALUES(salary);
```

**When to use:** ETL pipelines, syncing data, ensuring idempotent writes.

---

## 21. Views

A saved query that acts like a virtual table.

```sql
-- create a view
CREATE VIEW dept_summary AS
SELECT department,
    COUNT(*)    AS headcount,
    AVG(salary) AS avg_salary,
    MIN(salary) AS min_salary,
    MAX(salary) AS max_salary
FROM employees
GROUP BY department;

-- use it like a table
SELECT * FROM dept_summary WHERE headcount > 5;

-- update or replace
CREATE OR REPLACE VIEW dept_summary AS
SELECT department,
    COUNT(*)    AS headcount,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;

-- drop
DROP VIEW dept_summary;
```

### Materialized View (PostgreSQL)

Stores the result physically. Must be manually refreshed.

```sql
CREATE MATERIALIZED VIEW monthly_revenue AS
SELECT DATE_TRUNC('month', order_date) AS month,
       SUM(price * quantity) AS revenue
FROM orders
GROUP BY DATE_TRUNC('month', order_date);

-- refresh when underlying data changes
REFRESH MATERIALIZED VIEW monthly_revenue;
```

**When to use:** Simplifying complex queries, enforcing a consistent calculation, providing an abstraction layer.

---

## 22. Indexes

Speed up data retrieval at the cost of slower writes and more storage.

```sql
-- single column index
CREATE INDEX idx_emp_department ON employees (department);

-- composite index (order matters)
CREATE INDEX idx_orders_cust_date ON orders (customer_id, order_date);

-- unique index
CREATE UNIQUE INDEX idx_cust_email ON customers (email);

-- partial index (PostgreSQL) — only index active rows
CREATE INDEX idx_active_orders ON orders (order_date)
WHERE status = 'active';

-- drop index
DROP INDEX idx_emp_department;
```

### When to Create an Index

- Columns in WHERE, JOIN, or ORDER BY clauses
- Foreign key columns
- Columns with high cardinality (many distinct values)

### When NOT to Index

- Small tables (full scan is faster)
- Columns with low cardinality (e.g., boolean)
- Columns that are frequently updated
- Already heavily indexed tables (writes slow down)

---

## 23. Transactions

Group multiple operations into an atomic unit — all succeed or all fail.

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;

-- if both succeed
COMMIT;

-- if something went wrong
ROLLBACK;
```

### SAVEPOINT

```sql
BEGIN TRANSACTION;

UPDATE employees SET salary = salary * 1.1 WHERE department = 'Engineering';
SAVEPOINT after_engineering;

UPDATE employees SET salary = salary * 1.1 WHERE department = 'Sales';

-- oops, undo only the Sales update
ROLLBACK TO SAVEPOINT after_engineering;

COMMIT;  -- Engineering raises are kept
```

**When to use:** Any multi-step operation that must be all-or-nothing (fund transfers, order processing, batch updates).

---

## 24. Constraints

Rules enforced by the database to maintain data integrity.

```sql
CREATE TABLE products (
    product_id    INT PRIMARY KEY,                         -- uniquely identifies each row
    name          VARCHAR(100) NOT NULL,                   -- cannot be NULL
    sku           VARCHAR(50) UNIQUE,                      -- no duplicate SKUs
    price         DECIMAL(10,2) CHECK (price > 0),         -- must be positive
    category_id   INT REFERENCES categories(category_id),  -- foreign key
    created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP       -- default value
);

-- add constraint to existing table
ALTER TABLE employees
ADD CONSTRAINT fk_manager
FOREIGN KEY (manager_id) REFERENCES employees(id);

-- drop constraint
ALTER TABLE employees DROP CONSTRAINT fk_manager;
```

### Constraint Types

| Constraint | Purpose |
|-----------|---------|
| PRIMARY KEY | Unique identifier, not null, one per table |
| UNIQUE | No duplicate values (NULLs allowed) |
| NOT NULL | Column cannot contain NULL |
| CHECK | Value must satisfy a condition |
| FOREIGN KEY | Value must exist in referenced table |
| DEFAULT | Value used when none is provided |

---

## 25. Temporary Tables and Table Variables

### Temporary Tables

```sql
-- PostgreSQL / MySQL
CREATE TEMPORARY TABLE temp_high_earners AS
SELECT * FROM employees WHERE salary > 100000;

-- SQL Server (# prefix)
SELECT * INTO #temp_high_earners
FROM employees WHERE salary > 100000;

-- use it
SELECT department, COUNT(*) FROM temp_high_earners GROUP BY department;

-- cleanup (auto-dropped at session end, but good practice)
DROP TABLE IF EXISTS temp_high_earners;
```

### CTE vs Temp Table

| CTE | Temp Table |
|-----|-----------|
| Exists only for one statement | Persists for the session |
| Cannot be indexed | Can be indexed |
| Good for readability | Good for performance with large intermediate results |
| Computed every time referenced | Computed once, reused |

**When to use:** Staging data in ETL, storing intermediate results that are referenced multiple times, breaking complex transformations into steps.

---

## 26. Pivoting and Unpivoting

### Pivot with CASE (works everywhere)

Convert rows to columns.

```sql
-- sales per product per quarter
SELECT product,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 1 THEN price * quantity ELSE 0 END) AS Q1,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 2 THEN price * quantity ELSE 0 END) AS Q2,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 3 THEN price * quantity ELSE 0 END) AS Q3,
    SUM(CASE WHEN EXTRACT(QUARTER FROM order_date) = 4 THEN price * quantity ELSE 0 END) AS Q4
FROM orders
GROUP BY product;
```

### PIVOT (SQL Server)

```sql
SELECT *
FROM (
    SELECT department, YEAR(hire_date) AS hire_year, id
    FROM employees
) src
PIVOT (
    COUNT(id) FOR hire_year IN ([2021], [2022], [2023], [2024])
) pvt;
```

### Unpivot — Convert Columns to Rows

```sql
-- with UNION ALL (works everywhere)
SELECT product, 'Q1' AS quarter, q1_sales AS sales FROM product_summary
UNION ALL
SELECT product, 'Q2', q2_sales FROM product_summary
UNION ALL
SELECT product, 'Q3', q3_sales FROM product_summary
UNION ALL
SELECT product, 'Q4', q4_sales FROM product_summary;

-- SQL Server UNPIVOT
SELECT product, quarter, sales
FROM product_summary
UNPIVOT (sales FOR quarter IN (q1_sales, q2_sales, q3_sales, q4_sales)) unpvt;
```

**When to use:** Reporting, dashboards, transforming between wide and long data formats.

---

## 27. Recursive CTEs

A CTE that references itself. Used for hierarchical or graph data.

### Syntax

```sql
WITH RECURSIVE cte_name AS (
    -- anchor member: starting rows
    SELECT ...
    UNION ALL
    -- recursive member: references cte_name
    SELECT ... FROM cte_name WHERE <termination condition>
)
SELECT * FROM cte_name;
```

### Org Chart / Manager Hierarchy

```sql
WITH RECURSIVE org_chart AS (
    -- anchor: top-level (no manager)
    SELECT id, name, manager_id, 1 AS level,
           CAST(name AS VARCHAR(500)) AS path
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- recursive: find reports
    SELECT e.id, e.name, e.manager_id, oc.level + 1,
           CAST(oc.path || ' > ' || e.name AS VARCHAR(500))
    FROM employees e
    INNER JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT id, name, level, path
FROM org_chart
ORDER BY path;
```

### Generate a Number Series

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 100
)
SELECT n FROM numbers;
```

### Generate a Date Series

```sql
WITH RECURSIVE dates AS (
    SELECT DATE '2024-01-01' AS dt
    UNION ALL
    SELECT dt + INTERVAL '1 day' FROM dates WHERE dt < '2024-12-31'
)
SELECT dt FROM dates;
```

**When to use:** Traversing tree/graph structures (org charts, categories, bill of materials), generating sequences. Use `WITH RECURSIVE` in PostgreSQL/MySQL; SQL Server uses just `WITH` (recursive is implied).

---

## 28. Query Execution Order

Understanding execution order helps debug queries and write correct filters.

```
1. FROM        — identify and join tables
2. WHERE       — filter rows
3. GROUP BY    — group rows
4. HAVING      — filter groups
5. SELECT      — compute expressions and aliases
6. DISTINCT    — remove duplicates
7. ORDER BY    — sort results
8. LIMIT       — restrict output rows
```

### Key Implications

| Fact | Implication |
|------|-------------|
| WHERE runs before GROUP BY | WHERE cannot use aggregate functions |
| HAVING runs after GROUP BY | HAVING can use aggregate functions |
| SELECT runs after WHERE | Column aliases from SELECT can't be used in WHERE |
| ORDER BY runs after SELECT | Column aliases CAN be used in ORDER BY |
| WHERE runs before SELECT | Prefer filtering in WHERE over HAVING for performance |

```sql
-- this WON'T work: alias not available in WHERE
SELECT salary * 12 AS annual_salary
FROM employees
WHERE annual_salary > 100000;  -- ERROR

-- fix: repeat the expression
SELECT salary * 12 AS annual_salary
FROM employees
WHERE salary * 12 > 100000;

-- or use a subquery / CTE
WITH computed AS (
    SELECT *, salary * 12 AS annual_salary FROM employees
)
SELECT * FROM computed WHERE annual_salary > 100000;
```

---

## Quick Reference: Common Patterns

### Top-N Per Group

```sql
WITH ranked AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rn
    FROM employees
)
SELECT * FROM ranked WHERE rn <= 3;
```

### Running Total

```sql
SELECT order_date, price * quantity AS revenue,
    SUM(price * quantity) OVER (ORDER BY order_date) AS cumulative
FROM orders;
```

### Year-over-Year Comparison

```sql
WITH yearly AS (
    SELECT EXTRACT(YEAR FROM order_date) AS yr,
           SUM(price * quantity) AS revenue
    FROM orders
    GROUP BY EXTRACT(YEAR FROM order_date)
)
SELECT yr, revenue,
    LAG(revenue) OVER (ORDER BY yr) AS prev_year,
    ROUND((revenue - LAG(revenue) OVER (ORDER BY yr)) / LAG(revenue) OVER (ORDER BY yr) * 100, 1) AS yoy_pct
FROM yearly;
```

### Deduplication

```sql
-- keep only the latest row per customer
DELETE FROM orders
WHERE order_id NOT IN (
    SELECT MIN(order_id) FROM (
        SELECT order_id, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS rn
        FROM orders
    ) t WHERE rn = 1
);
```

### Gap Detection

```sql
-- find missing IDs
WITH all_ids AS (
    SELECT generate_series(MIN(id), MAX(id)) AS id FROM employees
)
SELECT a.id AS missing_id
FROM all_ids a
LEFT JOIN employees e ON a.id = e.id
WHERE e.id IS NULL;
```

### Conditional Aggregation (Pivot-Style)

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) FILTER (WHERE status = 'completed') AS completed,  -- PostgreSQL
    COUNT(*) FILTER (WHERE status = 'pending')   AS pending,
    COUNT(*) FILTER (WHERE status = 'cancelled') AS cancelled
FROM orders
GROUP BY DATE_TRUNC('month', order_date);
```

---

## 29. Data Types Reference

Choosing the right data type matters for storage, performance, and correctness.

### Numeric Types

| Type | Description | Range / Precision | Use For |
|------|-------------|-------------------|---------|
| `SMALLINT` | 2 bytes | -32,768 to 32,767 | Status codes, small counts |
| `INT` / `INTEGER` | 4 bytes | -2.1B to 2.1B | Primary keys, counts, IDs |
| `BIGINT` | 8 bytes | ±9.2 quintillion | Large IDs, timestamps as epoch |
| `DECIMAL(p,s)` / `NUMERIC(p,s)` | Exact precision | User-defined | Money, financial calculations |
| `FLOAT` / `REAL` | Approximate | ~7 digits (REAL), ~15 digits (FLOAT) | Scientific data, coordinates |
| `DOUBLE PRECISION` | 8-byte float | ~15 significant digits | Scientific calculations |
| `SERIAL` / `BIGSERIAL` | Auto-increment (PostgreSQL) | Same as INT/BIGINT | Auto-generated primary keys |
| `IDENTITY` | Auto-increment (SQL Server) | Same as INT/BIGINT | Auto-generated primary keys |

```sql
-- DECIMAL vs FLOAT — why it matters
SELECT CAST(0.1 + 0.2 AS FLOAT);           -- 0.30000000000000004 (imprecise)
SELECT CAST(0.1 AS DECIMAL(10,2)) + CAST(0.2 AS DECIMAL(10,2));  -- 0.30 (exact)

-- rule: use DECIMAL for money, FLOAT for science
CREATE TABLE transactions (
    amount DECIMAL(12,2) NOT NULL   -- NOT FLOAT
);
```

### String Types

| Type | Description | Use For |
|------|-------------|---------|
| `CHAR(n)` | Fixed-length, padded with spaces | Country codes, status codes (exactly n chars) |
| `VARCHAR(n)` | Variable-length, up to n chars | Names, emails, most text |
| `TEXT` | Unlimited length | Long descriptions, JSON strings, logs |
| `NVARCHAR(n)` (SQL Server) | Unicode variable-length | International text |

```sql
-- CHAR pads, VARCHAR does not
SELECT LENGTH(CAST('hi' AS CHAR(10)));     -- 10 (padded)
SELECT LENGTH(CAST('hi' AS VARCHAR(10)));  -- 2  (not padded)
```

### Date/Time Types

| Type | Description | Example |
|------|-------------|---------|
| `DATE` | Date only | `2024-03-15` |
| `TIME` | Time only | `14:30:00` |
| `TIMESTAMP` | Date + time (no timezone) | `2024-03-15 14:30:00` |
| `TIMESTAMPTZ` (PostgreSQL) | Date + time + timezone | `2024-03-15 14:30:00+05:30` |
| `DATETIME` (MySQL/SQL Server) | Date + time | `2024-03-15 14:30:00` |
| `INTERVAL` (PostgreSQL) | Duration | `INTERVAL '2 hours 30 minutes'` |

```sql
-- always store timestamps with timezone for global apps
CREATE TABLE events (
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Boolean

```sql
-- PostgreSQL / MySQL
CREATE TABLE features (
    is_active BOOLEAN DEFAULT TRUE
);

-- SQL Server (no native BOOLEAN)
CREATE TABLE features (
    is_active BIT DEFAULT 1   -- 0 or 1
);
```

### Other Useful Types

| Type | Database | Use For |
|------|----------|---------|
| `UUID` | PostgreSQL | Globally unique identifiers |
| `JSONB` | PostgreSQL | Structured semi-schema data |
| `JSON` | MySQL / PostgreSQL | JSON storage |
| `ARRAY` | PostgreSQL | Lists of values in a single column |
| `ENUM` | MySQL / PostgreSQL | Restricted set of values |
| `BYTEA` / `VARBINARY` | PostgreSQL / SQL Server | Binary data |

```sql
-- PostgreSQL UUID
CREATE TABLE sessions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id INT NOT NULL
);

-- PostgreSQL ARRAY
CREATE TABLE tags (
    id SERIAL PRIMARY KEY,
    labels TEXT[] DEFAULT '{}'
);
INSERT INTO tags (labels) VALUES (ARRAY['urgent', 'backend']);
SELECT * FROM tags WHERE 'urgent' = ANY(labels);
```

---

## 30. ALTER TABLE — Schema Changes

Modify existing tables without dropping them.

### Add / Drop / Rename Columns

```sql
-- add a column
ALTER TABLE employees ADD COLUMN email VARCHAR(100);

-- add with a default
ALTER TABLE employees ADD COLUMN is_active BOOLEAN DEFAULT TRUE;

-- drop a column
ALTER TABLE employees DROP COLUMN email;

-- rename a column (PostgreSQL)
ALTER TABLE employees RENAME COLUMN name TO full_name;

-- rename a column (SQL Server)
EXEC sp_rename 'employees.name', 'full_name', 'COLUMN';

-- rename a column (MySQL)
ALTER TABLE employees CHANGE name full_name VARCHAR(100);
```

### Modify Column Type

```sql
-- PostgreSQL
ALTER TABLE employees ALTER COLUMN salary TYPE DECIMAL(12,2);

-- MySQL
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12,2);

-- SQL Server
ALTER TABLE employees ALTER COLUMN salary DECIMAL(12,2);
```

### Add / Drop Constraints

```sql
-- add NOT NULL (PostgreSQL)
ALTER TABLE employees ALTER COLUMN email SET NOT NULL;

-- drop NOT NULL (PostgreSQL)
ALTER TABLE employees ALTER COLUMN email DROP NOT NULL;

-- add CHECK constraint
ALTER TABLE employees ADD CONSTRAINT chk_salary CHECK (salary > 0);

-- add foreign key
ALTER TABLE orders ADD CONSTRAINT fk_customer
FOREIGN KEY (customer_id) REFERENCES customers(customer_id);

-- drop constraint
ALTER TABLE orders DROP CONSTRAINT fk_customer;
```

### Rename Table

```sql
-- PostgreSQL / MySQL
ALTER TABLE employees RENAME TO staff;

-- SQL Server
EXEC sp_rename 'employees', 'staff';
```

**When to use:** Schema evolution — adding fields for new features, fixing data types, adding constraints to enforce data quality in production.

---

## 31. GROUPING SETS, ROLLUP, and CUBE

Generate multiple levels of aggregation in a single query — instead of writing multiple GROUP BY queries and UNION-ing them.

### GROUPING SETS

Explicitly define which groupings to compute.

```sql
-- get total revenue by: (1) product, (2) status, and (3) overall
SELECT product, status, SUM(price * quantity) AS revenue
FROM orders
GROUP BY GROUPING SETS (
    (product),          -- total per product
    (status),           -- total per status
    ()                  -- grand total
);
```

### ROLLUP

Produces subtotals from the most detailed to the grand total. Order matters.

```sql
-- subtotals: department → department+year → grand total
SELECT department, EXTRACT(YEAR FROM hire_date) AS hire_year, COUNT(*) AS headcount
FROM employees
GROUP BY ROLLUP (department, EXTRACT(YEAR FROM hire_date));

-- output includes:
-- (Engineering, 2023, 5)    ← detail
-- (Engineering, 2024, 3)    ← detail
-- (Engineering, NULL, 8)    ← subtotal for Engineering
-- (Sales, 2023, 4)          ← detail
-- (Sales, NULL, 4)          ← subtotal for Sales
-- (NULL, NULL, 12)          ← grand total
```

### CUBE

Produces subtotals for every possible combination of the grouping columns.

```sql
SELECT department, status, COUNT(*) AS cnt
FROM employees
GROUP BY CUBE (department, status);

-- output includes:
-- (Engineering, Active, 5)    ← detail
-- (Engineering, NULL, 8)      ← subtotal by department
-- (NULL, Active, 10)          ← subtotal by status
-- (NULL, NULL, 15)            ← grand total
```

### GROUPING() — Distinguish NULLs from Subtotal Rows

```sql
SELECT
    CASE WHEN GROUPING(department) = 1 THEN 'ALL DEPARTMENTS' ELSE department END AS department,
    CASE WHEN GROUPING(status) = 1 THEN 'ALL STATUSES' ELSE status END AS status,
    COUNT(*) AS cnt
FROM employees
GROUP BY CUBE (department, status);
```

### ROLLUP vs CUBE vs GROUPING SETS

| Feature | Combinations for (A, B, C) |
|---------|---------------------------|
| ROLLUP | (A,B,C), (A,B), (A), () — hierarchical |
| CUBE | (A,B,C), (A,B), (A,C), (B,C), (A), (B), (C), () — all combos |
| GROUPING SETS | Only the combos you list |

**When to use:** Reports with subtotals, dashboards with drill-downs, summary tables in data warehouses. Avoids writing multiple GROUP BY queries.

---

## 32. LATERAL Joins

A LATERAL join lets the subquery on the right side reference columns from the left side — like a correlated subquery, but in the FROM clause. Runs the subquery once per row from the left table.

### PostgreSQL: LATERAL

```sql
-- top 3 most recent orders per customer
SELECT c.name, recent.order_id, recent.product, recent.order_date
FROM customers c
CROSS JOIN LATERAL (
    SELECT o.order_id, o.product, o.order_date
    FROM orders o
    WHERE o.customer_id = c.customer_id
    ORDER BY o.order_date DESC
    LIMIT 3
) recent;

-- expand an array into rows
SELECT e.name, t.tag
FROM employees e
CROSS JOIN LATERAL unnest(e.skills) AS t(tag);
```

### SQL Server: CROSS APPLY / OUTER APPLY

`CROSS APPLY` = LATERAL with INNER JOIN behavior (skips rows with no match).
`OUTER APPLY` = LATERAL with LEFT JOIN behavior (keeps rows, NULLs for no match).

```sql
-- top 3 orders per customer (skip customers with no orders)
SELECT c.name, recent.order_id, recent.product
FROM customers c
CROSS APPLY (
    SELECT TOP 3 o.order_id, o.product, o.order_date
    FROM orders o
    WHERE o.customer_id = c.customer_id
    ORDER BY o.order_date DESC
) recent;

-- all customers, even those with no orders
SELECT c.name, recent.order_id, recent.product
FROM customers c
OUTER APPLY (
    SELECT TOP 3 o.order_id, o.product, o.order_date
    FROM orders o
    WHERE o.customer_id = c.customer_id
    ORDER BY o.order_date DESC
) recent;
```

### LATERAL vs Correlated Subquery

| LATERAL / APPLY | Correlated Subquery |
|-----------------|---------------------|
| Can return multiple rows and columns | Returns only one value (scalar) |
| Appears in FROM clause | Appears in SELECT or WHERE |
| Can use LIMIT/TOP per outer row | Cannot limit per outer row easily |

**When to use:** Top-N per group, expanding arrays/JSON, calling table-valued functions per row.

---

## 33. ANY, ALL, and SOME Operators

Compare a value against a set of values returned by a subquery.

### ANY / SOME (synonyms)

Returns TRUE if the condition is true for at least one value.

```sql
-- employees earning more than ANY person in HR (i.e., more than the lowest HR salary)
SELECT name, salary
FROM employees
WHERE salary > ANY (SELECT salary FROM employees WHERE department = 'HR');

-- equivalent to:
WHERE salary > (SELECT MIN(salary) FROM employees WHERE department = 'HR');
```

### ALL

Returns TRUE if the condition is true for every value.

```sql
-- employees earning more than ALL people in HR (i.e., more than the highest HR salary)
SELECT name, salary
FROM employees
WHERE salary > ALL (SELECT salary FROM employees WHERE department = 'HR');

-- equivalent to:
WHERE salary > (SELECT MAX(salary) FROM employees WHERE department = 'HR');
```

### Practical Comparison

```sql
-- = ANY is the same as IN
WHERE department = ANY (SELECT ...) -- same as IN (SELECT ...)

-- <> ALL is the same as NOT IN (but safer with NULLs)
WHERE department <> ALL (SELECT ...) -- same as NOT IN (SELECT ...)
```

**When to use:** Comparing against a dynamic set with operators other than `=`. For equality checks, `IN` is more readable.

---

## 34. JSON Functions

Modern databases treat JSON as a first-class data type. Essential for semi-structured data, API payloads, and event logs.

### PostgreSQL (JSONB)

```sql
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    payload JSONB NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

INSERT INTO events (payload) VALUES
('{"user": "alice", "action": "login", "metadata": {"ip": "10.0.0.1", "browser": "Chrome"}}');

-- extract a field (returns JSON)
SELECT payload -> 'user' FROM events;                  -- "alice" (with quotes)

-- extract a field as text
SELECT payload ->> 'user' FROM events;                 -- alice (without quotes)

-- nested extraction
SELECT payload -> 'metadata' ->> 'ip' FROM events;     -- 10.0.0.1

-- extract with path
SELECT payload #>> '{metadata, browser}' FROM events;   -- Chrome

-- filter by JSON field
SELECT * FROM events WHERE payload ->> 'action' = 'login';

-- check if key exists
SELECT * FROM events WHERE payload ? 'user';

-- check if JSON contains another JSON
SELECT * FROM events WHERE payload @> '{"action": "login"}';

-- expand JSON object to rows
SELECT key, value
FROM events, jsonb_each(payload -> 'metadata');

-- aggregate to JSON
SELECT jsonb_agg(jsonb_build_object('name', name, 'salary', salary))
FROM employees
WHERE department = 'Engineering';

-- update a JSON field
UPDATE events
SET payload = jsonb_set(payload, '{metadata, browser}', '"Firefox"')
WHERE id = 1;
```

### MySQL (JSON)

```sql
CREATE TABLE events (
    id INT AUTO_INCREMENT PRIMARY KEY,
    payload JSON NOT NULL
);

-- extract (returns JSON)
SELECT JSON_EXTRACT(payload, '$.user') FROM events;
SELECT payload -> '$.user' FROM events;                 -- shorthand

-- extract as text (unquoted)
SELECT JSON_UNQUOTE(JSON_EXTRACT(payload, '$.user')) FROM events;
SELECT payload ->> '$.user' FROM events;                -- MySQL 8.0+

-- nested
SELECT payload ->> '$.metadata.ip' FROM events;

-- search
SELECT * FROM events WHERE JSON_CONTAINS(payload, '"login"', '$.action');

-- check key exists
SELECT * FROM events WHERE JSON_CONTAINS_PATH(payload, 'one', '$.user');

-- modify
UPDATE events SET payload = JSON_SET(payload, '$.metadata.browser', 'Firefox') WHERE id = 1;

-- array operations
SELECT JSON_ARRAYAGG(name) FROM employees WHERE department = 'Engineering';
SELECT JSON_OBJECTAGG(name, salary) FROM employees;
```

### SQL Server (JSON as NVARCHAR with functions)

```sql
-- extract values
SELECT JSON_VALUE(payload, '$.user') FROM events;              -- scalar value
SELECT JSON_QUERY(payload, '$.metadata') FROM events;          -- JSON object/array

-- filter
SELECT * FROM events WHERE JSON_VALUE(payload, '$.action') = 'login';

-- check if valid JSON
SELECT ISJSON(payload) FROM events;

-- parse JSON array to rows
SELECT j.*
FROM events
CROSS APPLY OPENJSON(payload, '$.items')
WITH (
    product VARCHAR(100) '$.name',
    quantity INT '$.qty',
    price DECIMAL(10,2) '$.price'
) j;

-- build JSON
SELECT name, salary
FROM employees
WHERE department = 'Engineering'
FOR JSON PATH;
-- output: [{"name":"Alice","salary":95000},{"name":"Bob","salary":88000}]
```

### Indexing JSON (PostgreSQL)

```sql
-- GIN index for containment queries (@>, ?, ?|, ?&)
CREATE INDEX idx_events_payload ON events USING GIN (payload);

-- expression index for specific fields
CREATE INDEX idx_events_action ON events ((payload ->> 'action'));
```

**When to use:** Event stores, API logs, configuration storage, any semi-structured data. Use JSONB in PostgreSQL for best performance; avoid storing relational data as JSON when a proper table works.

---

## 35. Statistical and Advanced Window Functions

Beyond RANK and ROW_NUMBER — these functions are used for statistical analysis, percentile calculations, and cohort analysis.

### PERCENT_RANK

Relative rank as a percentage (0 to 1).

```sql
SELECT name, salary,
    PERCENT_RANK() OVER (ORDER BY salary) AS pct_rank
FROM employees;

-- output:
-- salary=40000 → pct_rank=0.0     (lowest)
-- salary=50000 → pct_rank=0.25
-- salary=90000 → pct_rank=0.75
-- salary=100000 → pct_rank=1.0    (highest)
```

### CUME_DIST

Cumulative distribution — fraction of rows with values <= current row.

```sql
SELECT name, salary,
    CUME_DIST() OVER (ORDER BY salary) AS cum_dist
FROM employees;

-- "what percentage of employees earn this much or less?"
-- salary=40000 → 0.2  (20% earn ≤40k)
-- salary=90000 → 0.8  (80% earn ≤90k)
```

### PERCENTILE_CONT / PERCENTILE_DISC

Calculate percentile values within a group.

```sql
-- PostgreSQL: median salary per department
SELECT department,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median_salary,
    PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY salary) AS p90_salary
FROM employees
GROUP BY department;

-- PERCENTILE_DISC returns an actual value from the dataset
-- PERCENTILE_CONT interpolates between values

-- as a window function (PostgreSQL)
SELECT name, department, salary,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary)
        OVER (PARTITION BY department) AS dept_median
FROM employees;

-- SQL Server: within a window
SELECT DISTINCT department,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary)
        OVER (PARTITION BY department) AS median_salary
FROM employees;
```

### Window Frame Deep Dive: ROWS vs RANGE vs GROUPS

```sql
-- ROWS: physical rows
SUM(salary) OVER (ORDER BY hire_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
-- always exactly the previous 2 rows + current

-- RANGE: logical range based on value
SUM(salary) OVER (ORDER BY hire_date RANGE BETWEEN INTERVAL '30 days' PRECEDING AND CURRENT ROW)
-- all rows with hire_date within 30 days before current row's hire_date

-- GROUPS: peer groups (rows with equal ORDER BY value count as one group)
SUM(salary) OVER (ORDER BY hire_date GROUPS BETWEEN 1 PRECEDING AND CURRENT ROW)
-- current group + 1 preceding group

-- frame boundaries:
-- UNBOUNDED PRECEDING    — first row of partition
-- n PRECEDING            — n rows/range/groups before current
-- CURRENT ROW            — current row
-- n FOLLOWING            — n rows/range/groups after current
-- UNBOUNDED FOLLOWING    — last row of partition

-- practical: 7-day rolling average revenue
SELECT order_date, SUM(price * quantity) AS daily_revenue,
    AVG(SUM(price * quantity)) OVER (
        ORDER BY order_date
        RANGE BETWEEN INTERVAL '6 days' PRECEDING AND CURRENT ROW
    ) AS rolling_7d_avg
FROM orders
GROUP BY order_date;
```

### NTH_VALUE

Get the value from the Nth row in the window frame.

```sql
-- second-highest salary per department
SELECT name, department, salary,
    NTH_VALUE(salary, 2) OVER (
        PARTITION BY department
        ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS second_highest
FROM employees;
```

---

## 36. EXPLAIN — Reading Query Plans

Understanding what the database does with your query is essential for performance.

### Basic Usage

```sql
-- PostgreSQL
EXPLAIN SELECT * FROM employees WHERE department = 'Engineering';
EXPLAIN ANALYZE SELECT * FROM employees WHERE department = 'Engineering';  -- actually runs it

-- MySQL
EXPLAIN SELECT * FROM employees WHERE department = 'Engineering';
EXPLAIN ANALYZE SELECT * FROM employees WHERE department = 'Engineering';  -- MySQL 8.0+

-- SQL Server
SET SHOWPLAN_TEXT ON;
GO
SELECT * FROM employees WHERE department = 'Engineering';
GO
SET SHOWPLAN_TEXT OFF;

-- SQL Server: graphical plan
SET STATISTICS PROFILE ON;
```

### PostgreSQL EXPLAIN Output

```sql
EXPLAIN ANALYZE
SELECT e.name, d.avg_salary
FROM employees e
JOIN (SELECT department, AVG(salary) AS avg_salary FROM employees GROUP BY department) d
    ON e.department = d.department
WHERE e.salary > d.avg_salary;

-- sample output:
-- Hash Join  (cost=1.50..3.25 rows=5 width=40) (actual time=0.05..0.08 rows=3 loops=1)
--   Hash Cond: (e.department = d.department)
--   Filter: (e.salary > d.avg_salary)
--   ->  Seq Scan on employees e  (cost=0.00..1.10 rows=10 width=72) (actual time=0.01..0.02 rows=10 loops=1)
--   ->  Hash  (cost=1.20..1.20 rows=3 width=40) (actual time=0.02..0.02 rows=3 loops=1)
--         ->  Subquery Scan on d  (cost=1.00..1.20 rows=3 width=40)
--               ->  HashAggregate  (cost=1.00..1.15 rows=3 width=40)
```

### What to Look For

| Node Type | Meaning | Good or Bad? |
|-----------|---------|-------------|
| Seq Scan | Reads every row in the table | Bad on large tables — add an index |
| Index Scan | Uses an index | Good |
| Index Only Scan | Answered from index alone | Best |
| Bitmap Index Scan | Combines multiple indexes | Good for OR conditions |
| Hash Join | Builds hash table, probes it | Good for large joins |
| Nested Loop | Inner loop per outer row | Good for small result sets |
| Merge Join | Both sides sorted, then merged | Good for large sorted datasets |
| Sort | Sorts data (may spill to disk) | Watch for large sorts |

### Key Metrics

```
cost=0.00..1.10     startup cost..total cost (arbitrary units)
rows=10             estimated number of rows
width=72            estimated row width in bytes
actual time=0.01    real milliseconds (only with ANALYZE)
loops=1             number of times this node ran
```

### Common Fixes

```sql
-- problem: Seq Scan on large table
-- fix: add index
CREATE INDEX idx_emp_dept ON employees (department);

-- problem: Sort node with large cost
-- fix: add index matching the ORDER BY
CREATE INDEX idx_orders_date ON orders (order_date DESC);

-- problem: Nested Loop with high loops count
-- fix: ensure join column is indexed
CREATE INDEX idx_orders_customer ON orders (customer_id);

-- force PostgreSQL to show if indexes are being used
SET enable_seqscan = off;  -- for testing only, don't use in production
```

**When to use:** Any query that runs slowly, before deploying new queries to production, validating that indexes are being used.

---

## 37. Performance Patterns and Anti-Patterns

### Anti-Patterns to Avoid

#### 1. Functions on Indexed Columns (Kills Index Usage)

```sql
-- BAD: index on salary won't be used
SELECT * FROM employees WHERE YEAR(hire_date) = 2024;

-- GOOD: rewrite as range
SELECT * FROM employees WHERE hire_date >= '2024-01-01' AND hire_date < '2025-01-01';

-- BAD
SELECT * FROM customers WHERE LOWER(email) = 'alice@example.com';

-- GOOD: use expression index (PostgreSQL)
CREATE INDEX idx_email_lower ON customers (LOWER(email));
SELECT * FROM customers WHERE LOWER(email) = 'alice@example.com';
```

#### 2. SELECT * in Production Queries

```sql
-- BAD: fetches all columns, prevents index-only scans
SELECT * FROM orders WHERE customer_id = 42;

-- GOOD: only what you need
SELECT order_id, product, order_date FROM orders WHERE customer_id = 42;
```

#### 3. N+1 Query Pattern

```sql
-- BAD (application code): one query per customer
-- for each customer:
--   SELECT * FROM orders WHERE customer_id = ?

-- GOOD: single query with JOIN
SELECT c.name, o.order_id, o.product
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE c.country = 'USA';
```

#### 4. Implicit Type Conversion

```sql
-- BAD: comparing string column to integer forces conversion per row
SELECT * FROM employees WHERE id = '42';

-- GOOD: use the correct type
SELECT * FROM employees WHERE id = 42;
```

#### 5. OR on Different Columns

```sql
-- BAD: can't use a single index effectively
SELECT * FROM employees WHERE department = 'Engineering' OR salary > 100000;

-- BETTER: UNION of two indexed queries
SELECT * FROM employees WHERE department = 'Engineering'
UNION
SELECT * FROM employees WHERE salary > 100000;
```

#### 6. NOT IN with NULLable Columns

```sql
-- BAD: returns zero rows if any NULL exists in subquery
SELECT * FROM customers WHERE customer_id NOT IN (SELECT customer_id FROM orders);

-- GOOD
SELECT * FROM customers c
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

### Performance Patterns

#### 1. Covering Indexes

```sql
-- an index that contains all columns needed by the query (index-only scan)
CREATE INDEX idx_orders_covering ON orders (customer_id, order_date, product);

-- this query can be answered entirely from the index, no table lookup needed
SELECT order_date, product FROM orders WHERE customer_id = 42;
```

#### 2. Batch Operations

```sql
-- BAD: insert one row at a time in a loop
INSERT INTO logs (msg) VALUES ('event 1');
INSERT INTO logs (msg) VALUES ('event 2');
-- ...1000 more

-- GOOD: batch insert
INSERT INTO logs (msg) VALUES ('event 1'), ('event 2'), ... ('event 1000');
```

#### 3. EXISTS Instead of COUNT for Existence Checks

```sql
-- BAD: counts ALL matching rows just to check if any exist
IF (SELECT COUNT(*) FROM orders WHERE customer_id = 42) > 0 ...

-- GOOD: stops at first match
IF EXISTS (SELECT 1 FROM orders WHERE customer_id = 42) ...
```

#### 4. Pagination with Keyset (Seek) Instead of OFFSET

```sql
-- BAD: OFFSET scans and discards rows — gets slower as page number grows
SELECT * FROM orders ORDER BY order_id LIMIT 20 OFFSET 10000;

-- GOOD: keyset pagination — constant performance
SELECT * FROM orders WHERE order_id > 10000 ORDER BY order_id LIMIT 20;
```

#### 5. Materialize Expensive CTEs

```sql
-- if a CTE is referenced multiple times and is expensive, use a temp table
CREATE TEMPORARY TABLE tmp_stats AS
SELECT department, AVG(salary) AS avg_sal FROM employees GROUP BY department;

CREATE INDEX idx_tmp_dept ON tmp_stats (department);

-- now use tmp_stats multiple times with index support
SELECT e.name, t.avg_sal
FROM employees e JOIN tmp_stats t ON e.department = t.department;
```

---

## Quick Reference: Common Patterns

### Top-N Per Group

```sql
WITH ranked AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rn
    FROM employees
)
SELECT * FROM ranked WHERE rn <= 3;
```

### Running Total

```sql
SELECT order_date, price * quantity AS revenue,
    SUM(price * quantity) OVER (ORDER BY order_date) AS cumulative
FROM orders;
```

### Year-over-Year Comparison

```sql
WITH yearly AS (
    SELECT EXTRACT(YEAR FROM order_date) AS yr,
           SUM(price * quantity) AS revenue
    FROM orders
    GROUP BY EXTRACT(YEAR FROM order_date)
)
SELECT yr, revenue,
    LAG(revenue) OVER (ORDER BY yr) AS prev_year,
    ROUND((revenue - LAG(revenue) OVER (ORDER BY yr)) / LAG(revenue) OVER (ORDER BY yr) * 100, 1) AS yoy_pct
FROM yearly;
```

### Deduplication

```sql
-- keep only the latest row per customer
DELETE FROM orders
WHERE order_id NOT IN (
    SELECT MIN(order_id) FROM (
        SELECT order_id, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS rn
        FROM orders
    ) t WHERE rn = 1
);
```

### Gap Detection

```sql
-- find missing IDs
WITH all_ids AS (
    SELECT generate_series(MIN(id), MAX(id)) AS id FROM employees
)
SELECT a.id AS missing_id
FROM all_ids a
LEFT JOIN employees e ON a.id = e.id
WHERE e.id IS NULL;
```

### Conditional Aggregation (Pivot-Style)

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) FILTER (WHERE status = 'completed') AS completed,  -- PostgreSQL
    COUNT(*) FILTER (WHERE status = 'pending')   AS pending,
    COUNT(*) FILTER (WHERE status = 'cancelled') AS cancelled
FROM orders
GROUP BY DATE_TRUNC('month', order_date);
```

### Slowly Changing Dimensions (SCD Type 2)

```sql
-- track historical changes to a record
CREATE TABLE customer_history (
    customer_id   INT,
    name          VARCHAR(100),
    city          VARCHAR(50),
    valid_from    DATE NOT NULL,
    valid_to      DATE,             -- NULL = current record
    is_current    BOOLEAN DEFAULT TRUE
);

-- close old record and insert new one when customer moves
UPDATE customer_history
SET valid_to = CURRENT_DATE, is_current = FALSE
WHERE customer_id = 42 AND is_current = TRUE;

INSERT INTO customer_history (customer_id, name, city, valid_from, is_current)
VALUES (42, 'Alice', 'Chicago', CURRENT_DATE, TRUE);

-- query: what city was customer 42 in on 2023-06-15?
SELECT * FROM customer_history
WHERE customer_id = 42
  AND valid_from <= '2023-06-15'
  AND (valid_to IS NULL OR valid_to > '2023-06-15');
```

### Data Quality Checks

```sql
-- find duplicate rows
SELECT name, email, COUNT(*) AS cnt
FROM customers
GROUP BY name, email
HAVING COUNT(*) > 1;

-- find orphaned records (foreign key integrity check)
SELECT o.*
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;

-- find NULL rates per column
SELECT
    COUNT(*) AS total,
    COUNT(*) - COUNT(email) AS email_nulls,
    ROUND((COUNT(*) - COUNT(email)) * 100.0 / COUNT(*), 1) AS email_null_pct,
    COUNT(*) - COUNT(phone) AS phone_nulls,
    ROUND((COUNT(*) - COUNT(phone)) * 100.0 / COUNT(*), 1) AS phone_null_pct
FROM customers;

-- check referential integrity before loading
SELECT DISTINCT source.customer_id
FROM staging_orders source
LEFT JOIN customers target ON source.customer_id = target.customer_id
WHERE target.customer_id IS NULL;
```

---

*End of reference. Bookmark this file and search by heading when you need a specific pattern.*
