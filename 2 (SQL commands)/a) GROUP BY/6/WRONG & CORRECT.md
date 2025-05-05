# Common Mistakes in DBMS When Using GROUP BY and HAVING

This document outlines **10 common mistakes** often made when using SQL queries that include both `GROUP BY` and `HAVING` clauses.

---

### 1. Using `HAVING` Without `GROUP BY` (When Not Using Aggregates)

```sql
SELECT department, COUNT(*)
FROM employees
HAVING COUNT(*) > 5;
```

❌ *Mistake*: `HAVING` used without grouping.

✅ *Fix*:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

### 2. Using `WHERE` Instead of `HAVING` for Aggregate Filters

```sql
SELECT department, COUNT(*)
FROM employees
WHERE COUNT(*) > 5
GROUP BY department;
```

❌ *Mistake*: `WHERE` can't filter on aggregate functions.

✅ *Fix*:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

### 3. Referencing Columns Not in `GROUP BY` or Aggregated

```sql
SELECT department, salary
FROM employees
GROUP BY department
HAVING salary > 50000;
```

❌ *Mistake*: `salary` is not grouped or aggregated.

✅ *Fix*:

```sql
SELECT department, MAX(salary) AS max_salary
FROM employees
GROUP BY department
HAVING MAX(salary) > 50000;
```

---

### 4. Incorrect Column Aliases in `HAVING` Clause

```sql
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
HAVING total > 5;
```

❌ *Mistake*: Aliases from `SELECT` can't be used in `HAVING`.

✅ *Fix*:

```sql
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

---

### 5. Not Understanding `HAVING` Applies After Grouping

```sql
SELECT department, COUNT(*)
FROM employees
HAVING department = 'Sales'
GROUP BY department;
```

❌ *Mistake*: Filtering grouped values incorrectly.

✅ *Fix*:

```sql
SELECT department, COUNT(*)
FROM employees
WHERE department = 'Sales'
GROUP BY department;
```

---

### 6. Using `HAVING` Without Aggregates or Grouping Meaningfully

```sql
SELECT name
FROM employees
GROUP BY name
HAVING name = 'John';
```

❌ *Mistake*: Unnecessary use of `GROUP BY` and `HAVING`.

✅ *Fix*:

```sql
SELECT name
FROM employees
WHERE name = 'John';
```

---

### 7. Using Multiple Aggregates Incorrectly in `HAVING`

```sql
SELECT department, AVG(salary), SUM(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) AND SUM(salary) > 100000;
```

❌ *Mistake*: Illogical condition.

✅ *Fix*:

```sql
SELECT department, AVG(salary), SUM(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 30000 AND SUM(salary) > 100000;
```

---

### 8. Grouping by Wrong or Incomplete Columns

```sql
SELECT department, job_title, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 10;
```

❌ *Mistake*: `job_title` is in `SELECT` but not in `GROUP BY`.

✅ *Fix*:

```sql
SELECT department, job_title, COUNT(*)
FROM employees
GROUP BY department, job_title
HAVING COUNT(*) > 10;
```

---

### 9. Assuming `HAVING` Improves Performance Like `WHERE`

✅ *Clarification*: `HAVING` is evaluated *after* grouping. Use `WHERE` to filter rows *before* grouping when possible for better performance.

---

### 10. Using Aliases or Complex Expressions in the Wrong Order

```sql
SELECT department, SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING total_salary > 50000;
```

❌ *Mistake*: Alias `total_salary` is not valid in `HAVING`.

✅ *Fix*:

```sql
SELECT department, SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 50000;
```

---

By avoiding these common pitfalls, you can write cleaner, more accurate, and more efficient SQL queries involving `GROUP BY` and `HAVING` clauses.
