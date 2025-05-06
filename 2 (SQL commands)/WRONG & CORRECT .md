# WRONG vs CORRECT EXISTS Operator Examples

This document provides side-by-side comparisons of incorrect and corrected SQL examples using the `EXISTS` operator in SQL (DBMS).

---

### 1. **Missing Subquery**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS;
```

**Correct:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1);
```

---

### 2. **Subquery without SELECT**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS (employees);
```

**Correct:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments);
```

---

### 3. **Subquery returns multiple columns (misleading)**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT id, name FROM departments);
```

**Correct:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments);
```

---

### 4. **Non-correlated subquery misused**

**Wrong:**

```sql
SELECT * FROM employees e WHERE EXISTS (SELECT * FROM departments WHERE department_id = 10);
```

**Correct:**

```sql
SELECT * FROM employees e 
WHERE EXISTS (
    SELECT 1 FROM departments d 
    WHERE d.department_id = e.department_id
);
```

---

### 5. **Comparison with EXISTS**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS = (SELECT * FROM departments);
```

**Correct:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments);
```

---

### 6. **Using EXISTS with scalar value**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT COUNT(*) FROM departments);
```

**Correct:**

```sql
SELECT * FROM employees 
WHERE EXISTS (
    SELECT 1 FROM departments WHERE departments.department_id = employees.department_id
);
```

---

### 7. **Subquery lacks FROM clause**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 WHERE department_id = 10);
```

**Correct:**

```sql
SELECT * FROM employees 
WHERE EXISTS (
    SELECT 1 FROM departments WHERE departments.department_id = 10
);
```

---

### 8. **Misuse of EXISTS in SELECT clause**

**Wrong:**

```sql
SELECT EXISTS (SELECT * FROM departments) AS dept_exists, name FROM employees;
```

**Correct:**

```sql
SELECT name, 
    CASE 
        WHEN EXISTS (SELECT 1 FROM departments WHERE departments.department_id = employees.department_id)
        THEN 'Yes' ELSE 'No'
    END AS dept_exists
FROM employees;
```

---

### 9. **EXISTS in JOIN ON clause**

**Wrong:**

```sql
SELECT * FROM employees e JOIN departments d ON EXISTS (SELECT * FROM projects p WHERE p.dept_id = d.id);
```

**Correct:**

```sql
SELECT * FROM employees e
WHERE EXISTS (
    SELECT 1 FROM projects p
    WHERE p.dept_id = e.department_id
);
```

---

### 10. **Invalid nesting of EXISTS**

**Wrong:**

```sql
SELECT * FROM employees WHERE EXISTS ((SELECT * FROM departments));
```

**Correct:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments);
```

---

These examples highlight how to properly use the `EXISTS` operator by correcting common syntax and logic errors.
