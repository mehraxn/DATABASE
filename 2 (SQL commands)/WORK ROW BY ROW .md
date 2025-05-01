# SQL: When Does It Work Row-by-Row?

This document explains when SQL processes queries **row-by-row** and when it does not, with a focus on correlated subqueries and general command types.

---

## ✅ When SQL Works Row-by-Row

SQL processes **row-by-row** when using **correlated subqueries**. In this case, the subquery refers to columns from the **outer query**, causing the subquery to be **re-evaluated for each outer row**.

### Examples of Row-by-Row Execution:

- `WHERE EXISTS (SELECT ... WHERE inner_column = outer_column)`
- `WHERE column = (SELECT ... WHERE correlated)`
- `WHERE column IN (SELECT ... WHERE correlated)`
- `> ALL (SELECT ... WHERE correlated)`
- `UPDATE ... SET column = (SELECT ... WHERE correlated)`
- `DELETE ... WHERE EXISTS (SELECT ... WHERE correlated)`

### Rule:
> If the subquery uses a column from the outer query, it is **correlated** and triggers **row-by-row evaluation**.

---

## ❌ When SQL Does **Not** Work Row-by-Row

SQL works on **sets** (not per-row) when:

- The subquery is **not correlated** (i.e., does not reference the outer query).
- The operation is a **JOIN**, `GROUP BY`, `ORDER BY`, or `HAVING` clause.

### Examples of Set-Based Execution:

- `WHERE column IN (SELECT column FROM table)` (non-correlated)
- `WHERE column = (SELECT MAX(...) FROM table)` (non-correlated)
- `INNER JOIN`, `LEFT JOIN`, etc.
- `GROUP BY` and `HAVING` (aggregation over sets)
- `WITH` (CTE) clauses (unless used inside correlated logic)
- Subqueries in `FROM (...)` (derived tables)

---

## ✅ Summary Table

| Subquery Type      | References Outer Query? | Row-by-Row? |
|--------------------|--------------------------|-------------|
| Correlated         | ✅ Yes                  | ✅ Yes    |
| Non-Correlated     | ❌ No                   | ❌ No     |
| JOIN               | ❌ No                   | ❌ No     |

---

## 🔍 Quick Tip

To spot row-by-row behavior, look inside the subquery:
- If it contains something like `WHERE inner_table.column = outer_table.column`, it's a correlated subquery and runs row-by-row.

If not, it's probably a single execution.

---

Use this guide to optimize and understand query performance and behavior.

