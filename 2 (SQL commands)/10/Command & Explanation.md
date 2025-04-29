# Understanding Nested SQL Queries in the Supply-Product Database

Nested SQL queries (also known as subqueries) are powerful tools for retrieving complex data from relational databases. This article examines several nested query examples using a Supply-Product database with three related tables:

## Database Schema

- **P (Products)**: PId (primary key), PName, Color, Size, Store
- **S (Suppliers)**: SId (primary key), SName, #Employees, City  
- **SP (Supplier-Product)**: Junction table with SId and PId as foreign keys, plus Qty

## Query Examples with Explanations

### 1. Find suppliers based in the same city as S1

```sql
SELECT SId 
FROM S 
WHERE City = (
    SELECT City 
    FROM S 
    WHERE SId='S1'
);
```

**Description**: This query finds all suppliers located in the same city as supplier S1. The subquery retrieves the city of S1, and the outer query returns all supplier IDs where the city matches that value.

### 2. Find names of suppliers who supply red products / who provide at least one red product

```sql
SELECT SName 
FROM S 
WHERE SId IN (
    SELECT SId 
    FROM SP 
    WHERE PId IN (
        SELECT PId 
        FROM P 
        WHERE Color='Red'
    )
);
```

**Description**: This query finds suppliers of red products or suppliers who provide at least one red product (these are equivalent descriptions of the same query). It works in three steps: first finding all red products, then identifying suppliers who supply those products, and finally retrieving those suppliers' names. This query can also be formulated using join operations:

```sql
SELECT DISTINCT S.SName 
FROM S, SP, P 
WHERE S.SId = SP.SId 
  AND SP.PId = P.PId 
  AND P.Color = 'Red';
```

### 3. Find names of suppliers who do not supply product P2

```sql
SELECT SName 
FROM S 
WHERE SId NOT IN (
    SELECT SId 
    FROM SP 
    WHERE PId='P2'
);
```

**Description**: This query returns names of suppliers who don't supply product P2. The subquery finds all suppliers who do supply P2, and the NOT IN operator excludes these suppliers from the results.

### 4. Find names of suppliers who only supply product P2

```sql
SELECT SName 
FROM S, SP 
WHERE S.SId NOT IN (
    SELECT SId 
    FROM SP 
    WHERE PId<>'P2'
) AND S.SId=SP.SId;
```

**Alternate version**:
```sql
SELECT SName 
FROM S 
WHERE S.SId NOT IN (
    SELECT SId 
    FROM SP 
    WHERE PId<>'P2'
) AND S.SId IN (
    SELECT SId 
    FROM SP
);
```

**Description**: These queries find suppliers who supply only product P2. They identify suppliers who don't supply any product other than P2 (using NOT IN) and also ensure they supply at least one product.

### 5. Find names of suppliers who do not supply any red products

```sql
SELECT SName 
FROM S 
WHERE SId NOT IN (
    SELECT SId 
    FROM SP 
    WHERE PId IN (
        SELECT PId 
        FROM P 
        WHERE Color='Red'
    )
);
```

**Description**: This query returns suppliers who don't supply any red products. It first identifies all red products, then finds suppliers who supply those products, and finally returns names of suppliers not in that group.

### 6. Find suppliers whose employee count is less than the maximum

```sql
SELECT SId 
FROM S 
WHERE #Employees < (
    SELECT MAX(#Employees) 
    FROM S
);
```

**Description**: This query finds suppliers with fewer employees than the supplier with the most employees. The subquery determines the maximum employee count across all suppliers, and the outer query returns suppliers with lower values.

### 7. Find names of suppliers who supply product P2

```sql
SELECT SName 
FROM S 
WHERE SId IN (
    SELECT SId 
    FROM SP 
    WHERE PId='P2'
);
```

**Description**: This query retrieves the names of all suppliers who supply product P2. The subquery identifies suppliers of P2, and the outer query returns their names.

### 8. Find names of suppliers who provide at least one red product

```sql
SELECT SName 
FROM S 
WHERE SId IN (
    SELECT SId 
    FROM SP 
    WHERE PId IN (
        SELECT PId 
        FROM P 
        WHERE Color='Red'
    )
);
```

**Description**: This query is identical to example #2, as both are finding suppliers who provide at least one red product. The innermost query identifies all red products, the middle query finds suppliers of those products, and the outer query retrieves the supplier names.

### 9. Find the codes of the suppliers whose number of employees is smaller than the maximum number of employees

```sql
SELECT SId 
FROM S 
WHERE #Employees < (
    SELECT MAX(#Employees) 
    FROM S
);
```

**Description**: This query finds suppliers with fewer employees than the supplier with the most employees. The subquery determines the maximum employee count across all suppliers, and the outer query returns suppliers with lower values. An equivalent formulation with join is not possible for this query.

## Conclusion

These examples demonstrate how nested queries can solve complex data retrieval problems by breaking them down into smaller, sequential operations. Nested queries are particularly useful for filtering data based on conditions across multiple tables in a relational database.