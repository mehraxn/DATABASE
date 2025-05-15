# Common Mistakes Using the `EXISTS` Command in SQL

This document outlines 10 common mistakes made when using the `EXISTS` command in SQL and provides their corrected forms.

---

### 1. **Mistake: Using `EXISTS` without a correlated subquery**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT * FROM departments);
```

**Corrected:**

```sql
SELECT * FROM employees e WHERE EXISTS (SELECT 1 FROM departments d WHERE d.manager_id = e.id);
```

---

### 2. **Mistake: Confusing `EXISTS` with `IN`**

```sql
SELECT name FROM employees WHERE department_id EXISTS (SELECT department_id FROM departments);
```

**Corrected:**

```sql
SELECT name FROM employees e WHERE EXISTS (SELECT 1 FROM departments d WHERE d.department_id = e.department_id);
```

---

### 3. **Mistake: Returning columns instead of constants in subquery**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT name FROM departments WHERE departments.id = employees.department_id);
```

**Corrected:**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments WHERE departments.id = employees.department_id);
```

---

### 4. **Mistake: Misplacing `EXISTS` logic in `JOIN` context**

```sql
SELECT * FROM employees e JOIN departments d ON EXISTS (SELECT 1 FROM locations);
```

**Corrected:**

```sql
SELECT * FROM employees e WHERE EXISTS (SELECT 1 FROM departments d WHERE d.id = e.department_id);
```

---

### 5. **Mistake: Not aliasing subquery tables properly**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments WHERE departments.manager_id = e.id);
```

**Corrected:**

```sql
SELECT * FROM employees e WHERE EXISTS (SELECT 1 FROM departments d WHERE d.manager_id = e.id);
```

---

### 6. **Mistake: Using `EXISTS` with an unrelated subquery**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM projects);
```

**Corrected:**

```sql
SELECT * FROM employees e WHERE EXISTS (SELECT 1 FROM projects p WHERE p.lead_id = e.id);
```

---

### 7. **Mistake: Using `EXISTS` when `JOIN` would be more appropriate**

```sql
SELECT name FROM employees WHERE EXISTS (SELECT 1 FROM departments WHERE departments.id = employees.department_id);
```

**Corrected:** (more efficient for fetching data)

```sql
SELECT e.name FROM employees e JOIN departments d ON d.id = e.department_id;
```

---

### 8. **Mistake: Using `NOT EXISTS` without handling nulls carefully**

```sql
SELECT * FROM employees WHERE NOT EXISTS (SELECT * FROM resignations WHERE resignations.emp_id = employees.id);
```

**Corrected:**

```sql
SELECT * FROM employees e WHERE NOT EXISTS (SELECT 1 FROM resignations r WHERE r.emp_id = e.id);
```

---

### 9. **Mistake: Assuming `EXISTS` filters duplicates**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT department_id FROM departments WHERE departments.id = employees.department_id);
```

**Corrected:** (EXISTS doesn't filter duplicates)

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM departments WHERE departments.id = employees.department_id);
```

---

### 10. **Mistake: Using `EXISTS` with unnecessary subquery**

```sql
SELECT * FROM employees WHERE EXISTS (SELECT 1 FROM dual);
```

**Corrected:** (if you want all employees, just write)

```sql
SELECT * FROM employees;
```

---

### Summary:

* `EXISTS` is best used for checking the presence of related rows.
* Always correlate the subquery with the outer query.
* Use `SELECT 1` in `EXISTS` subqueries for clarity and intent.
* Choose between `EXISTS`, `IN`, or `JOIN` based on use-case.

