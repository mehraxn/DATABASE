## Tables Used

Below are the tables assumed in the queries:

### Table: S (Suppliers)

| Column     | Type    | Description                |
| ---------- | ------- | -------------------------- |
| SId        | VARCHAR | Supplier ID (Primary Key)  |
| #Employees | INT     | Number of employees        |
| Country    | VARCHAR | Country of the supplier    |
| City       | VARCHAR | City where the supplier is |

### Table: Supplies

| Column | Type    | Description                    |
| ------ | ------- | ------------------------------ |
| SId    | VARCHAR | Supplier ID (Foreign Key to S) |
| PId    | VARCHAR | Product ID (Foreign Key to P)  |

### Table: P (Products)

| Column | Type    | Description              |
| ------ | ------- | ------------------------ |
| PId    | VARCHAR | Product ID (Primary Key) |
| PName  | VARCHAR | Product name             |
| Price  | FLOAT   | Price of the product     |

---

## SQL Query Scenarios

### 1. Suppliers with fewer employees than the average in USA

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT AVG(#Employees)
    FROM S
    WHERE Country = 'USA'
);
```

### 2. Suppliers with fewer employees than the minimum among those supplying 'P1'

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT MIN(#Employees)
    FROM S
    WHERE SId IN (
        SELECT SId
        FROM Supplies
        WHERE PId = 'P1'
    )
);
```

### 3. Suppliers with fewer employees than supplier 'S5'

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT #Employees
    FROM S
    WHERE SId = 'S5'
);
```

### 4. Suppliers with fewer employees than the one supplying the most distinct products

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT #Employees
    FROM S
    WHERE SId = (
        SELECT SId
        FROM (
            SELECT SId, COUNT(DISTINCT PId) AS ProductCount
            FROM Supplies
            GROUP BY SId
            ORDER BY ProductCount DESC
            LIMIT 1
        ) AS MostSupplying
    )
);
```

### 5. Suppliers with fewer employees than the average of those supplying products starting with 'A'

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT AVG(#Employees)
    FROM S
    WHERE SId IN (
        SELECT SId
        FROM Supplies
        WHERE PId IN (
            SELECT PId
            FROM P
            WHERE PName LIKE 'A%'
        )
    )
);
```

### 6. Suppliers with fewer employees than the max in their own city

```sql
SELECT S1.SId
FROM S AS S1
WHERE #Employees < (
    SELECT MAX(S2.#Employees)
    FROM S AS S2
    WHERE S2.City = S1.City
);
```

### 7. Suppliers with fewer employees than the one supplying the most expensive product

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT #Employees
    FROM S
    WHERE SId = (
        SELECT SId
        FROM Supplies
        WHERE PId = (
            SELECT PId
            FROM P
            ORDER BY Price DESC
            LIMIT 1
        )
        LIMIT 1
    )
);
```

### 8. Suppliers with fewer employees than any supplier in London

```sql
SELECT SId
FROM S
WHERE #Employees < ANY (
    SELECT #Employees
    FROM S
    WHERE City = 'London'
);
```

### 9. Suppliers with fewer employees than total employees of suppliers with more than 5 products

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT SUM(#Employees)
    FROM S
    WHERE SId IN (
        SELECT SId
        FROM Supplies
        GROUP BY SId
        HAVING COUNT(PId) > 5
    )
);
```

### 10. Suppliers with fewer employees than max in cities with more than one supplier

```sql
SELECT SId
FROM S
WHERE #Employees < (
    SELECT MAX(#Employees)
    FROM S
    WHERE City IN (
        SELECT City
        FROM S
        GROUP BY City
        HAVING COUNT(*) > 1
    )
);
```
