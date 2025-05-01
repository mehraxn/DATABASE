# SQL Subquery Analysis: Employee-Department-Project Database

## Database Schema

Based on the queries, we're working with a corporate database that includes information about employees, departments, and projects. Here are the tables:

### Table Employees:
| EmpID | Name      | DeptID | Salary  | HireDate   |
|-------|-----------|--------|---------|------------|
| 101   | John Smith| 1      | 75000   | 2018-05-15 |
| 102   | Mary Jones| 1      | 85000   | 2017-06-20 |
| 103   | Bob Wilson| 2      | 63000   | 2019-01-10 |
| 104   | Sue Davis | 2      | 72000   | 2018-11-05 |
| 105   | Tom Brown | 3      | 68000   | 2020-03-12 |
| 106   | Lisa Green| 3      | 68000   | 2019-08-15 |
| 107   | Kim Lee   | 4      | 92000   | 2016-12-08 |
| 108   | Ted Black | 4      | 85000   | 2018-04-22 |
| 109   | Joe White | 1      | 90000   | 2016-09-30 |
| 110   | Amy Martin| 2      | 72000   | 2019-05-16 |

### Table Departments:
| DeptID | DeptName      | ManagerID | Location      |
|--------|---------------|-----------|---------------|
| 1      | Engineering   | 109       | Building A    |
| 2      | Marketing     | 103       | Building B    |
| 3      | Human Resources| 105      | Building A    |
| 4      | Finance       | 107       | Building C    |
| 5      | Research      | 112       | Building D    |

### Table Projects:
| ProjectID | ProjectName    | DeptID | Budget  | StartDate  | EndDate    |
|-----------|----------------|--------|---------|------------|------------|
| 501       | Website Redesign| 1      | 80000   | 2023-01-15 | 2023-06-30 |
| 502       | Product Launch | 2      | 65000   | 2023-02-01 | 2023-05-15 |
| 503       | HRIS Update    | 3      | 50000   | 2023-03-10 | 2023-09-20 |
| 504       | Financial Audit| 4      | 95000   | 2023-01-05 | 2023-04-30 |
| 505       | Mobile App     | 1      | 120000  | 2023-04-01 | 2023-12-15 |
| 506       | Social Media   | 2      | 35000   | 2023-05-01 | 2023-08-31 |
| 507       | Benefits Review| 3      | 30000   | 2023-06-15 | 2023-10-15 |

Now let's analyze each query with these tables in mind.

## Query 1: Finding Highest Paid Employees in Each Department

```sql
SELECT Name, DeptID, Salary
FROM Employees E
WHERE Salary = (
    SELECT MAX(Salary)
    FROM Employees E2
    WHERE E2.DeptID = E.DeptID
);
```

### Explanation
This query finds the employees who earn the highest salary within their respective departments. For each employee in the outer query, the subquery calculates the maximum salary for that employee's department, and the WHERE clause filters to only include employees whose salary matches that maximum.

### Step-by-Step Logic
1. For each employee in the Employees table (aliased as E)
2. The subquery finds the maximum salary from all employees (E2) within the same department
3. The outer query returns employees whose salary equals this maximum
4. The result shows the name, department ID, and salary of the highest-paid employee in each department

### Output
| Name      | DeptID | Salary |
|-----------|--------|--------|
| Joe White | 1      | 90000  |
| Sue Davis | 2      | 72000  |
| Amy Martin| 2      | 72000  |
| Tom Brown | 3      | 68000  |
| Lisa Green| 3      | 68000  |
| Kim Lee   | 4      | 92000  |

Note: In Department 2, both Sue Davis and Amy Martin have the same salary (72000), which is the maximum for that department, so both appear in the results. Similarly, in Department 3, both employees have the same salary.

## Query 2: Finding Departments Where Employees Earn More Than Their Manager

```sql
SELECT DeptName
FROM Departments D
WHERE EXISTS (
    SELECT 1
    FROM Employees E
    WHERE E.DeptID = D.DeptID
      AND E.Salary > (
          SELECT Salary
          FROM Employees M
          WHERE M.EmpID = D.ManagerID
      )
);
```

### Explanation
This query identifies departments where at least one employee earns more than the department's manager. It uses nested subqueries with an EXISTS clause to check this condition.

### Step-by-Step Logic
1. For each department in the Departments table (aliased as D)
2. The outer subquery checks if there exists any employee (E) in that department
3. The inner subquery finds the salary of the manager of that department
4. The condition checks if the employee's salary is greater than the manager's salary
5. If such an employee exists, the department name is included in the results

### Output
| DeptName    |
|-------------|
| Engineering |
| Marketing   |

In our sample data, these departments have at least one employee earning more than their manager.

## Query 3: Finding Projects with Budgets Below Average Department Salary

```sql
SELECT ProjectID, DeptID, Budget
FROM Projects P
WHERE Budget < (
    SELECT AVG(Salary)
    FROM Employees E
    WHERE E.DeptID = P.DeptID
);
```

### Explanation
This query finds projects where the budget is less than the average salary of employees in the department managing that project. It compares each project's budget with the average salary of employees in the corresponding department.

### Step-by-Step Logic
1. For each project in the Projects table (aliased as P)
2. The subquery calculates the average salary of employees in the same department as the project
3. The outer query returns projects where the budget is less than this average salary
4. The result shows the project ID, department ID, and budget of qualifying projects

### Output
| ProjectID | DeptID | Budget |
|-----------|--------|--------|
| 503       | 3      | 50000  |
| 506       | 2      | 35000  |
| 507       | 3      | 30000  |

These projects have budgets lower than the average salary in their respective departments.

## Query 4: Finding Employees Not in Departments with Highest Budget Projects

```sql
SELECT Name
FROM Employees E
WHERE DeptID NOT IN (
    SELECT DeptID
    FROM Projects P1
    WHERE Budget = (
        SELECT MAX(Budget)
        FROM Projects P2
    )
);
```

### Explanation
This query finds employees who do not work in departments that have projects with the maximum budget across all projects. It uses a nested subquery to identify departments with the highest budget project, then excludes employees from those departments.

### Step-by-Step Logic
1. The innermost subquery finds the maximum budget across all projects
2. The middle subquery finds departments that have projects with this maximum budget
3. The main query returns names of employees whose department IDs are NOT in this list
4. The result shows the names of employees who don't work in departments with the highest-budget projects

### Output
| Name       |
|------------|
| Bob Wilson |
| Sue Davis  |
| Amy Martin |
| Tom Brown  |
| Lisa Green |
| Kim Lee    |
| Ted Black  |

Based on our sample data, the highest budget project is the Mobile App (120000) in Department 1, so this query returns employees who don't work in Department 1.

## Query 5: Finding Employees with Salaries Higher Than Any Project Budget in Their Department

```sql
SELECT Name
FROM Employees E
WHERE EXISTS (
    SELECT 1
    FROM Projects P
    WHERE P.DeptID = E.DeptID
      AND E.Salary > P.Budget
);
```

### Explanation
This query finds employees whose salary is greater than the budget of at least one project in their department. It uses an EXISTS subquery to check if there's any project in the employee's department with a budget less than the employee's salary.

### Step-by-Step Logic
1. For each employee in the Employees table (aliased as E)
2. The subquery checks if there exists any project (P) in the same department
3. Where the employee's salary is greater than the project's budget
4. If such a project exists, the employee's name is included in the results

### Output
| Name       |
|------------|
| Joe White  |
| Tom Brown  |
| Lisa Green |
| Kim Lee    |

These employees have salaries higher than at least one project budget in their department.