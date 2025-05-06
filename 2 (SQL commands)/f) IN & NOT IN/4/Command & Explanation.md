# SQL Query Equivalence: Two Approaches to Finding Top-Paid Sales Representatives

In database management, there are often multiple ways to write a SQL query that produces the same result. Understanding these equivalent approaches can help database administrators and developers choose the most efficient method for their specific scenarios. Today, we'll explore two equivalent SQL queries that identify departments where Sales Representatives earn more than $50,000.

## The Database Structure

Let's first understand the database we're working with. Consider a company database with the following structure:

**Employees Table:**
- employee_id (Primary Key)
- first_name
- last_name
- email
- phone_number
- hire_date
- job_title
- salary
- department_id (Foreign Key)

**Departments Table:**
- department_id (Primary Key)
- department_name
- location_id
- manager_id

This structure represents a typical corporate database where employees belong to specific departments, and each employee has a designated job title and salary.

## The Business Question

The marketing team wants to analyze which departments have Sales Representatives earning more than $50,000. This information will help them understand which departments are potentially more lucrative for sales staff and might inform future hiring decisions or compensation strategies.

## Approach 1: Using GROUP BY with HAVING

The first approach uses the HAVING clause, which is designed to filter groups after aggregation:

```sql
SELECT department_id, MAX(salary) AS highest_salary 
FROM employees 
WHERE job_title = 'Sales Representative' 
GROUP BY department_id 
HAVING MAX(salary) > 50000;
```

## Approach 2: Using a Subquery with WHERE

The second approach uses a subquery to create a derived table and then filters it with a WHERE clause:

```sql
SELECT department_id, highest_salary 
FROM (
    SELECT department_id, MAX(salary) AS highest_salary
    FROM employees
    WHERE job_title = 'Sales Representative'
    GROUP BY department_id
) AS dept_salaries 
WHERE highest_salary > 50000;
```

## Sample Data and Results

Let's populate our database with some sample data:

**Sample Employees Data:**
| employee_id | first_name | last_name | job_title             | salary  | department_id |
|-------------|------------|-----------|------------------------|---------|---------------|
| 101         | John       | Smith     | Sales Representative   | 52000   | 10            |
| 102         | Mary       | Johnson   | Sales Representative   | 48000   | 10            |
| 103         | Robert     | Williams  | Sales Representative   | 55000   | 20            |
| 104         | Susan      | Brown     | Sales Representative   | 51000   | 20            |
| 105         | Michael    | Jones     | Sales Representative   | 49000   | 30            |
| 106         | Patricia   | Miller    | Sales Representative   | 53000   | 40            |
| 107         | James      | Davis     | Marketing Specialist   | 54000   | 50            |
| 108         | Jennifer   | Garcia    | Sales Representative   | 47000   | 50            |
| 109         | David      | Rodriguez | Sales Representative   | 63000   | 60            |
| 110         | Lisa       | Wilson    | HR Specialist          | 58000   | 70            |

**Sample Departments Data:**
| department_id | department_name      | location_id |
|---------------|----------------------|-------------|
| 10            | North Sales          | 1000        |
| 20            | East Sales           | 2000        |
| 30            | West Sales           | 3000        |
| 40            | South Sales          | 4000        |
| 50            | Marketing            | 5000        |
| 60            | Executive Sales      | 6000        |
| 70            | Human Resources      | 7000        |

**Query Results (for both queries):**
| department_id | highest_salary |
|---------------|----------------|
| 10            | 52000          |
| 20            | 55000          |
| 40            | 53000          |
| 60            | 63000          |

The results show that departments 10, 20, 40, and 60 have Sales Representatives earning more than $50,000, with the highest salary being $63,000 in department 60 (Executive Sales).

## Comparing the Two Approaches

### Execution Flow

**Approach 1 (HAVING):**
1. Filter employees to only Sales Representatives
2. Group these employees by department_id
3. Calculate MAX(salary) for each group
4. Filter groups where MAX(salary) > $50,000
5. Return department_id and highest_salary

**Approach 2 (Subquery):**
1. Inner query: Filter employees to only Sales Representatives
2. Inner query: Group by department_id
3. Inner query: Calculate MAX(salary) as highest_salary for each group
4. Outer query: Filter the derived table where highest_salary > $50,000
5. Return department_id and highest_salary

### Performance Considerations

While both queries produce the same result, there might be slight performance differences depending on the database engine:

1. **HAVING Approach (Approach 1)**:
   - Typically more straightforward and easier to read
   - The database optimizes this common pattern well in most cases
   - May be slightly more efficient as it avoids creating a derived table

2. **Subquery Approach (Approach 2)**:
   - Creates an intermediate result set
   - May be useful when you need to apply multiple conditions to the aggregated data
   - Some database engines might optimize this differently
   - Provides more flexibility for complex scenarios

## When to Use Each Approach

**Use the HAVING approach when:**
- The query is straightforward with a single condition on the aggregated result
- You want more readable code
- Performance is critical and you want to avoid derived tables

**Use the Subquery approach when:**
- You need to apply multiple filters to the aggregated data
- The derived table will be reused in multiple parts of the query
- You're working with a database engine that optimizes this pattern well
- You need to perform additional operations on the aggregated data

## Conclusion

These two SQL approaches demonstrate the flexibility of SQL in solving business problems. While they produce identical results for our Sales Representative salary analysis, understanding both methods enriches your SQL toolkit.

For our specific example of finding departments with Sales Representatives earning over $50,000, either approach is valid. The choice might come down to team coding standards, database engine optimization preferences, or the need for query maintainability and readability.

As database professionals continue to work with increasingly complex data requirements, mastering these equivalent patterns becomes essential for writing efficient and effective SQL queries.