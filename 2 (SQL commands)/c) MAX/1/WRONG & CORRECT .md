# Common Mistakes with SQL MAX Operator and Their Fixes

Below is a list of 10 incorrect uses of the SQL `MAX()` operator along with their corrected versions and explanations.

---

### 1. **Missing FROM Clause**

**Wrong:**

```sql
SELECT MAX(salary);
```

**Correct:**

```sql
SELECT MAX(salary) FROM employees;
```

---

### 2. **MAX on Date Column Without Context**

**Wrong:**

```sql
SELECT MAX(hire_date) FROM employees;
```

**Correct:**

```sql
SELECT * FROM employees
WHERE hire_date = (SELECT MAX(hire_date) FROM employees);
```

---

### 3. **Missing GROUP BY Clause**

**Wrong:**

```sql
SELECT department_id, MAX(salary)
FROM employees;
```

**Correct:**

```sql
SELECT department_id, MAX(salary)
FROM employees
GROUP BY department_id;
```

---

### 4. **Using MAX in WHERE Clause**

**Wrong:**

```sql
SELECT * FROM employees
WHERE salary = MAX(salary);
```

**Correct:**

```sql
SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

---

### 5. **Using MAX in HAVING Without GROUP BY**

**Wrong:**

```sql
SELECT MAX(salary)
FROM employees
HAVING salary > 50000;
```

**Correct:**

```sql
SELECT MAX(salary)
FROM employees
WHERE salary > 50000;
```

---

### 6. **Incorrect Use of MAX with ORDER BY**

**Wrong:**

```sql
SELECT name, MAX(salary)
FROM employees
ORDER BY salary DESC;
```

**Correct (Option A):**

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC;
```

**Correct (Option B):**

```sql
SELECT name, salary
FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

---

### 7. **Using MAX in INSERT Without Subquery**

**Wrong:**

```sql
INSERT INTO top_earners (name, salary)
VALUES ('John', MAX(salary));
```

**Correct:**

```sql
INSERT INTO top_earners (name, salary)
SELECT 'John', MAX(salary) FROM employees;
```

---

### 8. **MAX with Multiple Columns**

**Wrong:**

```sql
SELECT MAX(salary, bonus) FROM employees;
```

**Correct:**

```sql
SELECT GREATEST(salary, bonus) FROM employees;
```

**Alternative (Individual MAX values):**

```sql
SELECT MAX(salary) AS max_salary, MAX(bonus) AS max_bonus FROM employees;
```

---

### 9. **Using DISTINCT Inside MAX**

**Wrong:**

```sql
SELECT MAX(DISTINCT salary) FROM employees;
```

**Correct:**

```sql
SELECT MAX(salary) FROM employees;
```

---

### 10. **MAX on NULL**

**Wrong:**

```sql
SELECT MAX(NULL) FROM employees;
```

**Correct:**

```sql
SELECT MAX(salary) FROM employees WHERE salary IS NOT NULL;
```

---

This guide can help prevent common logic and syntax errors when using `MAX()` in SQL queries.
