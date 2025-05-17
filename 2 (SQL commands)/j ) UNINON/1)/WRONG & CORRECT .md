# README: Common Mistakes When Using UNION and UNION ALL in SQL

This guide highlights 10 common mistakes developers make when using `UNION` and `UNION ALL`, along with their corrected forms and examples.

---

### 1. Mixing Columns with Different Data Types

**Mistake:**

```sql
SELECT name FROM employees
UNION
SELECT salary FROM payroll;
```

**Fix:** Ensure both columns are of compatible types.

```sql
SELECT CAST(name AS VARCHAR) FROM employees
UNION
SELECT CAST(salary AS VARCHAR) FROM payroll;
```

---

### 2. Using UNION When Duplicates Should Be Kept

**Mistake:**

```sql
SELECT city FROM customers
UNION
SELECT city FROM suppliers;
```

**Fix:** Use `UNION ALL` to retain duplicates.

```sql
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

---

### 3. Expecting UNION to Maintain Row Order

**Mistake:**

```sql
SELECT name FROM A
UNION
SELECT name FROM B
ORDER BY name DESC;
```

**Fix:** Place `ORDER BY` after all UNION parts.

```sql
(SELECT name FROM A
UNION
SELECT name FROM B)
ORDER BY name DESC;
```

---

### 4. Using ORDER BY in Each Subquery

**Mistake:**

```sql
SELECT name FROM A ORDER BY name
UNION
SELECT name FROM B ORDER BY name;
```

**Fix:** Use one `ORDER BY` at the end.

```sql
SELECT name FROM A
UNION
SELECT name FROM B
ORDER BY name;
```

---

### 5. Missing Parentheses in Complex Queries

**Mistake:**

```sql
SELECT name FROM A
UNION SELECT name FROM B WHERE age > 30;
```

**Fix:** Use parentheses to clarify logic.

```sql
(SELECT name FROM A)
UNION
(SELECT name FROM B WHERE age > 30);
```

---

### 6. Inconsistent Column Aliases

**Mistake:**

```sql
SELECT name AS full_name FROM A
UNION
SELECT name FROM B;
```

**Fix:** Use consistent aliases.

```sql
SELECT name AS full_name FROM A
UNION
SELECT name AS full_name FROM B;
```

---

### 7. Combining Semantically Different Data

**Mistake:**

```sql
SELECT product_name FROM products
UNION
SELECT supplier_name FROM suppliers;
```

**Fix:** Only union similar data.

```sql
SELECT customer_name FROM online_customers
UNION
SELECT customer_name FROM retail_customers;
```

---

### 8. Ignoring Performance of UNION vs UNION ALL

**Mistake:**

```sql
SELECT * FROM logs_2024
UNION
SELECT * FROM logs_2025;
```

**Fix:** Use `UNION ALL` if duplicates aren't a concern.

```sql
SELECT * FROM logs_2024
UNION ALL
SELECT * FROM logs_2025;
```

---

### 9. Not Handling NULLs Properly

**Mistake:**

```sql
SELECT phone FROM customers
UNION
SELECT NULL FROM dual;
```

**Fix:** Use `COALESCE` to standardize NULLs.

```sql
SELECT COALESCE(phone, 'Unknown') FROM customers
UNION
SELECT 'Unknown' FROM dual;
```

---

### 10. Using UNION Instead of JOIN

**Mistake:**

```sql
SELECT employee_id FROM employees
UNION
SELECT department_id FROM departments;
```

**Fix:** Use JOIN for relational merges.

```sql
SELECT e.employee_id, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id;
```

---

> 📌 Tip: `UNION` removes duplicates (slower), `UNION ALL` keeps all rows (faster).
