# Understanding IN and NOT IN in SQL and Their Relation to JOINs

## IN Operator
The `IN` operator in SQL is used to check whether a value exists in a set of values returned by a subquery or a list. It returns rows where the specified column matches one of the values in the list.

### Example:
```sql
SELECT name
FROM Employees
WHERE department_id IN (SELECT id FROM Departments WHERE location = 'New York');
```
This query returns all employees who work in departments located in New York.

### Equivalent Using JOIN:
```sql
SELECT e.name
FROM Employees e
JOIN Departments d ON e.department_id = d.id
WHERE d.location = 'New York';
```
This JOIN achieves the same result as the `IN` operator by matching records based on a common key.

## NOT IN Operator
The `NOT IN` operator is used to exclude rows where a column's value appears in a subquery or list. It returns rows where the value does *not* exist in the specified set.

### Example:
```sql
SELECT name
FROM Employees
WHERE department_id NOT IN (SELECT id FROM Departments WHERE location = 'New York');
```
This query returns all employees who do not work in departments located in New York.

## Why NOT IN Cannot Be Reliably Achieved with JOIN
Unlike `IN`, `NOT IN` cannot be safely rewritten using a JOIN due to the following reasons:

### 1. **Join Operation Retains Matches Only**
JOINs are designed to return rows where there is a match between tables. When trying to exclude matches (as `NOT IN` does), using a JOIN directly doesn't work because it inherently keeps only the matched rows.

### 2. **LEFT JOIN with IS NULL**
A common workaround is using a LEFT JOIN with a NULL check:
```sql
SELECT e.name
FROM Employees e
LEFT JOIN Departments d ON e.department_id = d.id AND d.location = 'New York'
WHERE d.id IS NULL;
```
This will return employees who do not work in a department located in New York.

### 3. **NULL Complications**
If the subquery used in `NOT IN` returns NULLs, the entire comparison may fail because `NOT IN` with NULL results in unknowns, and rows may be excluded unexpectedly. This makes it risky and unreliable to use a JOIN-based approach unless NULLs are properly handled.

### 4. **Preferred Alternative: NOT EXISTS**
Instead of using a JOIN for `NOT IN`, the recommended and safer approach is:
```sql
SELECT e.name
FROM Employees e
WHERE NOT EXISTS (
  SELECT 1 FROM Departments d WHERE d.id = e.department_id AND d.location = 'New York'
);
```
This explicitly ensures that no matching row exists in the subquery, and handles NULLs more predictably.

## Summary
| Operator | Can be Rewritten Using JOIN? | Notes |
|----------|------------------------------|-------|
| IN       | Yes                          | JOIN naturally includes matching rows |
| NOT IN   | Not reliably                 | JOIN cannot exclude matches without workarounds, NULLs complicate logic |

Use `NOT EXISTS` for safe and reliable exclusion logic instead of relying on JOINs for `NOT IN`.

