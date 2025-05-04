# SQL GROUP BY - Wrong vs Correct Usage Examples

This document presents common incorrect usages of the `GROUP BY` clause in SQL, followed by corrected and logically sound alternatives.

---

## 1. Grouping Without Aggregation

**Wrong:**

```sql
SELECT Name, Age
FROM Employees
GROUP BY Department;
```

**Why it's wrong:** Selecting columns that are neither grouped nor aggregated.

**Correct:**

```sql
SELECT Department, MAX(Age) AS OldestEmployeeAge
FROM Employees
GROUP BY Department;
```

**Explanation:** Aggregates `Age` per department.

---

## 2. Grouping by a Constant

**Wrong:**

```sql
SELECT COUNT(*)
FROM Employees
GROUP BY 1;
```

**Why it's wrong:** `GROUP BY 1` creates a single group unnecessarily.

**Correct:**

```sql
SELECT COUNT(*)
FROM Employees;
```

**Explanation:** No need to group; the result is a single count.

---

## 3. Grouping by a Primary Key

**Wrong:**

```sql
SELECT EmployeeID, COUNT(*)
FROM Employees
GROUP BY EmployeeID;
```

**Why it's wrong:** If `EmployeeID` is unique, `COUNT(*)` is always 1.

**Correct:**

```sql
SELECT ManagerID, COUNT(*) AS TeamSize
FROM Employees
GROUP BY ManagerID;
```

**Explanation:** Shows team sizes per manager.

---

## 4. Grouping by Unrelated Columns

**Wrong:**

```sql
SELECT ProductName, AVG(Salary)
FROM Products
GROUP BY ProductName;
```

**Why it's wrong:** `Salary` doesn't belong in the `Products` table.

**Correct:**

```sql
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department;
```

**Explanation:** Uses correct table and relevant columns.

---

## 5. Redundant GROUP BY

**Wrong:**

```sql
SELECT COUNT(*)
FROM Orders
GROUP BY NULL;
```

**Why it's wrong:** Grouping is unnecessary.

**Correct:**

```sql
SELECT COUNT(*)
FROM Orders;
```

**Explanation:** Simplified query with the same result.

---

These examples help clarify when and how to use `GROUP BY` meaningfully in SQL queries.
