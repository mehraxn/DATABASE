# SQL JOIN Operations - Results

## Our Example Tables

### Departments Table
| DeptID | DeptName   |
|--------|------------|
| 1      | IT         |
| 2      | HR         |
| 3      | Finance    |
| 4      | Marketing  |

### Employees Table
| EmpID | EmpName        | DeptID |
|-------|---------------|--------|
| 101   | John Smith     | 1      |
| 102   | Sarah Connor   | 1      |
| 103   | Michael Brown  | 2      |
| 104   | Emily Davis    | 3      |
| 105   | David Wilson   | NULL   |
| 106   | Lisa Johnson   | NULL   |

## 1. INNER JOIN Results
```sql
SELECT E.EmpID, E.EmpName, D.DeptName
FROM Employees E
INNER JOIN Departments D ON E.DeptID = D.DeptID;
```

| EmpID | EmpName        | DeptName |
|-------|---------------|----------|
| 101   | John Smith     | IT       |
| 102   | Sarah Connor   | IT       |
| 103   | Michael Brown  | HR       |
| 104   | Emily Davis    | Finance  |

**Explanation:** INNER JOIN returns only matching rows. Only employees with valid department IDs and departments with employees are included. Notice that employees with NULL DeptID and the Marketing department (which has no employees) are excluded.

## 2. LEFT OUTER JOIN Results
```sql
SELECT E.EmpID, E.EmpName, D.DeptName
FROM Employees E
LEFT OUTER JOIN Departments D ON E.DeptID = D.DeptID;
```

| EmpID | EmpName        | DeptName |
|-------|---------------|----------|
| 101   | John Smith     | IT       |
| 102   | Sarah Connor   | IT       |
| 103   | Michael Brown  | HR       |
| 104   | Emily Davis    | Finance  |
| 105   | David Wilson   | NULL     |
| 106   | Lisa Johnson   | NULL     |

**Explanation:** LEFT JOIN returns all rows from the Employees table (left) and matched rows from the Departments table (right). Employees without departments show NULL in the DeptName column.

## 3. RIGHT OUTER JOIN Results
```sql
SELECT E.EmpID, E.EmpName, D.DeptName
FROM Employees E
RIGHT OUTER JOIN Departments D ON E.DeptID = D.DeptID;
```

| EmpID | EmpName        | DeptName   |
|-------|---------------|------------|
| 101   | John Smith     | IT         |
| 102   | Sarah Connor   | IT         |
| 103   | Michael Brown  | HR         |
| 104   | Emily Davis    | Finance    |
| NULL  | NULL          | Marketing  |

**Explanation:** RIGHT JOIN returns all rows from the Departments table (right) and matched rows from the Employees table (left). The Marketing department has no employees, so it shows NULL values for employee data.

## 4. FULL OUTER JOIN Results
```sql
SELECT E.EmpID, E.EmpName, D.DeptName
FROM Employees E
FULL OUTER JOIN Departments D ON E.DeptID = D.DeptID;
```

| EmpID | EmpName        | DeptName   |
|-------|---------------|------------|
| 101   | John Smith     | IT         |
| 102   | Sarah Connor   | IT         |
| 103   | Michael Brown  | HR         |
| 104   | Emily Davis    | Finance    |
| 105   | David Wilson   | NULL       |
| 106   | Lisa Johnson   | NULL       |
| NULL  | NULL          | Marketing  |

**Explanation:** FULL OUTER JOIN returns all rows from both tables. When there's no match, NULL values appear in the columns from the table without a match. This includes employees with no department and departments with no employees.