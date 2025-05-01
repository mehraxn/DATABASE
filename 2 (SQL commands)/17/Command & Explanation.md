# README: SQL Subquery Analysis: Exploring Complex Database Operations

## Database Schema

Our database consists of three main tables:

### Table S (Suppliers):
| SId | SName | City    |
|-----|-------|---------|
| S1  | Smith | London  |
| S2  | Jones | Paris   |
| S3  | Blake | Paris   |
| S4  | Clark | London  |
| S5  | Adams | Athens  |

### Table P (Products):
| PId | PName  | Color  | Weight | City    |
|-----|--------|--------|--------|---------|
| P1  | Nut    | Red    | 12     | London  |
| P2  | Bolt   | Green  | 17     | Paris   |
| P3  | Screw  | Blue   | 17     | Rome    |
| P4  | Screw  | Red    | 14     | London  |
| P5  | Cam    | Blue   | 12     | Paris   |
| P6  | Cog    | Red    | 19     | London  |

### Table SP (Supplier-Product):
| SId | PId | Qty | City    |
|-----|-----|-----|---------|
| S1  | P1  | 300 | London  |
| S1  | P2  | 200 | London  |
| S1  | P3  | 400 | Paris   |
| S1  | P4  | 200 | London  |
| S1  | P5  | 100 | Paris   |
| S1  | P6  | 100 | London  |
| S2  | P1  | 300 | Paris   |
| S2  | P2  | 400 | Paris   |
| S3  | P2  | 200 | Paris   |
| S4  | P2  | 200 | London  |
| S4  | P4  | 300 | London  |
| S4  | P5  | 400 | London  |

## Query 1: Finding Suppliers Who Supply Products in Their Own City

```sql
SELECT SName
FROM S
WHERE EXISTS (
    SELECT *
    FROM SP
    WHERE SP.SId = S.SId
      AND SP.City = S.City
);
```

### Explanation
This query searches for suppliers who have supplied at least one product in the same city where they are located. The `EXISTS` subquery checks if there's any record in the SP table where both the supplier ID matches and the city matches.

### Step-by-Step Logic
1. For each supplier in the S table, the query checks if there exists a supplier-product relationship
2. Where the supplier IDs match between the tables
3. AND where the cities also match between the tables
4. If such a relationship exists, the supplier's name is returned

### Output
| SName |
|-------|
| Smith |
| Jones |
| Clark |

In our sample data, these suppliers have provided products in cities where they are based.

## Query 2: Finding Maximum Quantities for Each Supplier-Product Combination

```sql
SELECT PId, SId, Qty
FROM SP AS SPX
WHERE Qty = (
    SELECT MAX(Qty)
    FROM SP AS SPY
    WHERE SPY.PId = SPX.PId
      AND SPY.SId = SPX.SId
);
```

### Explanation
This query is interesting in that it compares each row in the SP table with itself. It finds the maximum quantity for each unique product-supplier combination. Since a specific supplier-product combination will only have one maximum value (itself), this query will effectively return all rows from the SP table.

### Step-by-Step Logic
1. For each supplier-product relationship in SP (aliased as SPX)
2. The subquery finds the maximum quantity from SP (aliased as SPY)
3. Where the product ID and supplier ID are the same in both tables
4. The outer query returns rows where the quantity equals this maximum

### Output
All rows from the SP table would be returned, as each unique combination of SId and PId has only one maximum quantity value.

| SId | PId | Qty |
|-----|-----|-----|
| S1  | P1  | 300 |
| S1  | P2  | 200 |
| S1  | P3  | 400 |
| S1  | P4  | 200 |
| S1  | P5  | 100 |
| S1  | P6  | 100 |
| S2  | P1  | 300 |
| S2  | P2  | 400 |
| S3  | P2  | 200 |
| S4  | P2  | 200 |
| S4  | P4  | 300 |
| S4  | P5  | 400 |

## Query 3: Finding Suppliers Who Haven't Supplied Product P2 in Their City

```sql
SELECT SName
FROM S
WHERE NOT EXISTS (
    SELECT *
    FROM SP
    WHERE SP.SId = S.SId
      AND SP.PId = 'P2'
      AND SP.City = S.City
);
```

### Explanation
This query finds suppliers who have NOT supplied product 'P2' in the same city where they are located. It uses the `NOT EXISTS` operator to exclude suppliers who have a matching entry in the SP table.

### Step-by-Step Logic
1. For each supplier in the S table, check if there does NOT exist a record in the SP table
2. Where the supplier IDs match between the tables
3. AND the product ID is specifically 'P2'
4. AND the cities match between the tables
5. If no such record exists, include the supplier's name in the results

### Output
| SName |
|-------|
| Blake |
| Adams |

These suppliers either haven't supplied P2 at all, or haven't supplied it in their home city.

## Query 4: Finding Suppliers with Large Orders in Their City

```sql
SELECT SName
FROM S
WHERE (SId, City) IN (
    SELECT SId, City
    FROM SP
    WHERE Qty > 300
);
```

### Explanation
This query finds suppliers who have supplied any product with a quantity greater than 300 in the same city where they are located. It uses an IN clause with a tuple comparison (SId, City).

### Step-by-Step Logic
1. The subquery finds all supplier ID and city combinations from the SP table where the quantity exceeds 300
2. The main query then checks if a supplier's ID and city appear in this result set
3. If they do, the supplier's name is included in the results

### Output
| SName |
|-------|
| Jones |
| Clark |

Based on our sample data, these suppliers have orders with quantities exceeding 300 in their home cities.

## Query 5: Finding Products with Uniform Weights by Color and City

```sql
SELECT PId
FROM P
WHERE Weight = ALL (
    SELECT Weight
    FROM P AS PX
    WHERE PX.Color = P.Color
      AND PX.City = P.City
);
```

### Explanation
This query finds products where, for a given color and city combination, all products have the same weight. It uses the `ALL` operator, which requires the condition to be true for all values returned by the subquery.

### Step-by-Step Logic
1. For each product in the P table
2. The subquery selects weights of all products (PX) with the same color and city
3. The main query checks if the product's weight equals ALL the weights returned by the subquery
4. If this condition is true, the product ID is included in the results

### Output
| PId |
|-----|
| P1  |
| P2  |
| P3  |
| P4  |
| P5  |
| P6  |

In our sample data, it appears that for each unique combination of color and city, all products have the same weight, so all product IDs are returned.