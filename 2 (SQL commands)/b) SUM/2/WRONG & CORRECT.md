# Common Mistakes with SQL SUM() Operator and Their Fixes

This document outlines 10 common mistakes made when using the SQL `SUM()` operator, along with corrected versions of each example.

---

## 1. Missing GROUP BY Clause

**Wrong:**

```sql
SELECT department, SUM(salary) FROM employees;
```

**Correct:**

```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY department;
```

---

## 2. Using SUM() in WHERE Clause

**Wrong:**

```sql
SELECT * FROM sales WHERE SUM(amount) > 1000;
```

**Correct:**

```sql
SELECT customer_id, SUM(amount) AS total
FROM sales
GROUP BY customer_id
HAVING SUM(amount) > 1000;
```

---

## 3. Using SUM() on Non-Numeric Column

**Wrong:**

```sql
SELECT SUM(order_date) FROM orders;
```

**Correct:**

```sql
-- Cannot sum dates. Instead, sum a numeric field like:
SELECT SUM(total_amount)
FROM orders;
```

---

## 4. Using SUM() Without an Argument

**Wrong:**

```sql
SELECT SUM() FROM payments;
```

**Correct:**

```sql
SELECT SUM(amount)
FROM payments;
```

---

## 5. Using Alias in HAVING Clause

**Wrong:**

```sql
SELECT customer_id, SUM(amount) as total FROM sales GROUP BY customer_id HAVING total > 500;
```

**Correct:**

```sql
SELECT customer_id, SUM(amount) AS total
FROM sales
GROUP BY customer_id
HAVING SUM(amount) > 500;
```

---

## 6. Using SUM() in UPDATE Clause

**Wrong:**

```sql
UPDATE employees SET salary = SUM(salary) WHERE department = 'Sales';
```

**Correct:**

```sql
-- Use a subquery to calculate SUM separately:
UPDATE employees
SET salary = (
    SELECT SUM(salary)
    FROM employees
    WHERE department = 'Sales'
)
WHERE department = 'Sales';
```

---

## 7. Incorrect DISTINCT Usage in SUM()

**Wrong:**

```sql
SELECT SUM(DISTINCT salary, bonus) FROM payroll;
```

**Correct:**

```sql
SELECT SUM(DISTINCT salary + bonus)
FROM payroll;
```

---

## 8. Using SUM() in ORDER BY Without Alias

**Wrong:**

```sql
SELECT department, SUM(salary) FROM employees GROUP BY department ORDER BY total;
```

**Correct:**

```sql
SELECT department, SUM(salary) AS total
FROM employees
GROUP BY department
ORDER BY total;
```

---

## 9. Subquery Returning Multiple Rows in SUM()

**Wrong:**

```sql
SELECT SUM((SELECT salary FROM employees)) FROM dual;
```

**Correct:**

```sql
SELECT SUM(salary)
FROM employees;
```

---

## 10. Mixing Aggregate and Non-Aggregate Without GROUP BY

**Wrong:**

```sql
SELECT employee_id, SUM(salary) FROM employees;
```

**Correct:**

```sql
SELECT employee_id, SUM(salary)
FROM employees
GROUP BY employee_id;
```
