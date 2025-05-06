# README: Examples of WRONG and CORRECT Usage of the IN Operator in SQL

This README provides 10 examples of incorrect (WRONG) and corrected (CORRECT) usage of the `IN` operator in SQL queries. Each example includes a brief note explaining the issue and how it was fixed.

---

### 1. Using a variable or column without parentheses

**WRONG:**

```sql
SELECT * FROM employees WHERE department_id IN department_list;
```

**CORRECT:**

```sql
SELECT * FROM employees WHERE department_id IN (101, 102, 103);
```

---

### 2. Missing closing parenthesis

**WRONG:**

```sql
SELECT name FROM customers WHERE country IN ('USA', 'Canada', 'Mexico';
```

**CORRECT:**

```sql
SELECT name FROM customers WHERE country IN ('USA', 'Canada', 'Mexico');
```

---

### 3. Mixed data types in list

**WRONG:**

```sql
SELECT * FROM products WHERE price IN (100, 200, 'Three Hundred');
```

**CORRECT:**

```sql
SELECT * FROM products WHERE price IN (100, 200, 300);
```

---

### 4. No parentheses around list

**WRONG:**

```sql
SELECT * FROM orders WHERE order_date IN '2023-01-01', '2023-01-02';
```

**CORRECT:**

```sql
SELECT * FROM orders WHERE order_date IN ('2023-01-01', '2023-01-02');
```

---

### 5. Subquery returns wrong column

**WRONG:**

```sql
SELECT * FROM books WHERE author_id IN (SELECT name FROM authors);
```

**CORRECT:**

```sql
SELECT * FROM books WHERE author_id IN (SELECT author_id FROM authors);
```

---

### 6. Empty IN list

**WRONG:**

```sql
SELECT * FROM sales WHERE region IN ();
```

**CORRECT:**

```sql
SELECT * FROM sales WHERE region IN ('North', 'South');
```

---

### 7. Single value without parentheses

**WRONG:**

```sql
SELECT * FROM users WHERE role IN 'admin';
```

**CORRECT:**

```sql
SELECT * FROM users WHERE role IN ('admin');
```

---

### 8. Subquery with multiple columns

**WRONG:**

```sql
SELECT * FROM inventory WHERE quantity IN (SELECT * FROM quantities);
```

**CORRECT:**

```sql
SELECT * FROM inventory WHERE quantity IN (SELECT quantity FROM quantities);
```

---

### 9. Including NULL in list

**WRONG:**

```sql
SELECT * FROM movies WHERE genre_id IN (1, 2, NULL);
```

**CORRECT:**

```sql
SELECT * FROM movies WHERE genre_id IN (1, 2) OR genre_id IS NULL;
```

---

### 10. Missing comma in list

**WRONG:**

```sql
SELECT * FROM students WHERE grade IN (90, 80 70);
```

**CORRECT:**

```sql
SELECT * FROM students WHERE grade IN (90, 80, 70);
```

---

This list can be used as a quick reference or for teaching purposes to illustrate proper use of the `IN` operator in SQL.
