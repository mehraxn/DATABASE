# Common Mistakes with IS \[NOT] NULL in SQL

This document lists common mistakes made when using `IS NULL` or `IS NOT NULL` in SQL, along with their corrected versions.

---

### 1. Using `= NULL` instead of `IS NULL`

**Wrong:**

```sql
SELECT * FROM employees WHERE manager_id = NULL;
```

**Fixed:**

```sql
SELECT * FROM employees WHERE manager_id IS NULL;
```

---

### 2. Using `<> NULL` instead of `IS NOT NULL`

**Wrong:**

```sql
SELECT * FROM employees WHERE salary <> NULL;
```

**Fixed:**

```sql
SELECT * FROM employees WHERE salary IS NOT NULL;
```

---

### 3. Checking NULL on computed expressions instead of source columns

**Wrong:**

```sql
SELECT * FROM orders WHERE (quantity * price) IS NULL;
```

**Fixed:**

```sql
SELECT * FROM orders WHERE quantity IS NULL OR price IS NULL;
```

---

### 4. Using aggregate functions with `IS NULL` in `WHERE`

**Wrong:**

```sql
SELECT department_id, COUNT(*)
FROM employees
WHERE COUNT(*) IS NOT NULL
GROUP BY department_id;
```

**Fixed:**

```sql
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id
HAVING COUNT(*) IS NOT NULL;
```

---

### 5. Checking NULL on NOT NULL columns

**Wrong:**

```sql
SELECT * FROM users WHERE user_id IS NULL;
```

**Fixed:**

```sql
-- Remove the condition if user_id is a primary key
SELECT * FROM users;
```

---

### 6. Using `IS NULL` on inner JOINs without LEFT JOIN

**Wrong:**

```sql
SELECT *
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE c.name IS NULL;
```

**Fixed:**

```sql
SELECT *
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.name IS NULL;
```

---

### 7. Misordered `NOT IS NULL`

**Wrong:**

```sql
SELECT * FROM products WHERE NOT IS NULL(price);
```

**Fixed:**

```sql
SELECT * FROM products WHERE price IS NOT NULL;
```

---

### 8. Misuse of `IN` with NULL

**Wrong:**

```sql
SELECT * FROM employees WHERE NULL IN (manager_id);
```

**Fixed:**

```sql
SELECT * FROM employees WHERE manager_id IS NULL;
```

---

### 9. Illogical combination of NULL check and comparison

**Wrong:**

```sql
SELECT * FROM invoices
WHERE due_date IS NULL AND due_date > '2024-01-01';
```

**Fixed:**

```sql
SELECT * FROM invoices
WHERE due_date IS NULL;
```

---

### 10. Misunderstanding IS NOT NULL in view logic

**Wrong:**

```sql
CREATE VIEW active_users AS
SELECT * FROM users
WHERE status = 'active' OR status IS NOT NULL;
```

**Fixed:**

```sql
CREATE VIEW active_users AS
SELECT * FROM users
WHERE status = 'active';
```
