# SQL Join Operations Analysis

## Database Schema

For these SQL join operations, we'll use the same database schema as in the first example. Here are the tables again:

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

Now let's analyze each join query with these tables.

## Query 1: Inner Join with Filter on Product Color

```sql
SELECT S.SName, P.PName
FROM P
INNER JOIN SP ON P.PId = SP.PId
INNER JOIN S ON S.SId = SP.SId
WHERE P.Color = 'Red';
```

### Explanation
This query retrieves supplier names and product names for all red products that have been supplied by any supplier. It uses INNER JOINs to connect the three tables, and filters for only red products.

### Step-by-Step Logic
1. Start with the Products table (P)
2. Join with the Supplier-Product table (SP) where product IDs match
3. Join with the Suppliers table (S) where supplier IDs match
4. Filter to include only products with a color of 'Red'
5. Return the supplier name and product name columns

### Output
| SName | PName |
|-------|-------|
| Smith | Nut   |
| Smith | Screw |
| Smith | Cog   |
| Jones | Nut   |
| Clark | Screw |

These are all suppliers who have supplied red products (Nut, Screw, and Cog).

## Query 2: Left Join of All Tables

```sql
SELECT P.PName, S.SName
FROM P
LEFT JOIN SP ON P.PId = SP.PId
LEFT JOIN S ON S.SId = SP.SId;
```

### Explanation
This query returns all products along with any matching suppliers. It uses LEFT JOINs, which means all products will be included in the results, even those that have not been supplied by any supplier.

### Step-by-Step Logic
1. Start with the Products table (P)
2. Left join with the Supplier-Product table (SP) where product IDs match
3. Left join with the Suppliers table (S) where supplier IDs match
4. Return the product name and supplier name columns

### Output
| PName | SName |
|-------|-------|
| Nut   | Smith |
| Nut   | Jones |
| Bolt  | Smith |
| Bolt  | Jones |
| Bolt  | Blake |
| Bolt  | Clark |
| Screw | Smith |
| Screw | Clark |
| Cam   | Smith |
| Cam   | Clark |
| Cog   | Smith |

All products are listed, along with the suppliers who supply them. If a product has multiple suppliers, it appears in multiple rows.

## Query 3: Right Join of All Tables

```sql
SELECT S.SName, P.PName
FROM P
RIGHT JOIN SP ON P.PId = SP.PId
RIGHT JOIN S ON S.SId = SP.SId;
```

### Explanation
This query returns all suppliers along with any matching products. It uses RIGHT JOINs, which means all suppliers will be included in the results, even those that do not supply any products.

### Step-by-Step Logic
1. Start with the Products table (P)
2. Right join with the Supplier-Product table (SP) where product IDs match
3. Right join with the Suppliers table (S) where supplier IDs match
4. Return the supplier name and product name columns

### Output
| SName | PName |
|-------|-------|
| Smith | Nut   |
| Smith | Bolt  |
| Smith | Screw |
| Smith | Screw |
| Smith | Cam   |
| Smith | Cog   |
| Jones | Nut   |
| Jones | Bolt  |
| Blake | Bolt  |
| Clark | Bolt  |
| Clark | Screw |
| Clark | Cam   |
| Adams | NULL  |

All suppliers are listed, along with the products they supply. If a supplier supplies multiple products, they appear in multiple rows. Adams doesn't supply any products and has NULL in the PName column.

## Query 4: Full Outer Join of All Tables

```sql
SELECT S.SName, P.PName
FROM S
FULL OUTER JOIN SP ON S.SId = SP.SId
FULL OUTER JOIN P ON P.PId = SP.PId;
```

### Explanation
This query returns all suppliers and all products, including cases where suppliers have no products or products have no suppliers. It uses FULL OUTER JOINs to include all records from all tables.

### Step-by-Step Logic
1. Start with the Suppliers table (S)
2. Full outer join with the Supplier-Product table (SP) where supplier IDs match
3. Full outer join with the Products table (P) where product IDs match
4. Return the supplier name and product name columns

### Output
| SName | PName |
|-------|-------|
| Smith | Nut   |
| Smith | Bolt  |
| Smith | Screw |
| Smith | Screw |
| Smith | Cam   |
| Smith | Cog   |
| Jones | Nut   |
| Jones | Bolt  |
| Blake | Bolt  |
| Clark | Bolt  |
| Clark | Screw |
| Clark | Cam   |
| Adams | NULL  |

This output includes all suppliers (even those who don't supply any products) and would also include any products that aren't supplied by any supplier (though in our sample data, all products are supplied by at least one supplier).

## Query 5: Inner Join with Multiple Filters

```sql
SELECT S.SName, P.PName
FROM S
INNER JOIN SP ON S.SId = SP.SId
INNER JOIN P ON P.PId = SP.PId
WHERE P.Color = 'Red' AND S.City = 'London';
```

### Explanation
This query finds suppliers in London who supply red products. It uses INNER JOINs to connect the tables and applies filters for both the product color and supplier city.

### Step-by-Step Logic
1. Start with the Suppliers table (S)
2. Join with the Supplier-Product table (SP) where supplier IDs match
3. Join with the Products table (P) where product IDs match
4. Filter to include only red products and suppliers in London
5. Return the supplier name and product name columns

### Output
| SName | PName |
|-------|-------|
| Smith | Nut   |
| Smith | Screw |
| Smith | Cog   |
| Clark | Screw |

These are suppliers in London who have supplied red products.

## Query 6: Left Join with Conditional Join

```sql
SELECT P.PName, S.SName
FROM P
LEFT JOIN SP ON P.PId = SP.PId
LEFT JOIN S ON S.SId = SP.SId AND S.City = 'Paris';
```

### Explanation
This query is interesting because it includes a condition in the JOIN clause itself. It finds all products and joins them with suppliers from Paris. Products that aren't supplied by Paris suppliers will still be included, but with NULL for the supplier name.

### Step-by-Step Logic
1. Start with the Products table (P)
2. Left join with the Supplier-Product table (SP) where product IDs match
3. Left join with the Suppliers table (S) where supplier IDs match AND the supplier is in Paris
4. Return the product name and supplier name columns

### Output
| PName | SName |
|-------|-------|
| Nut   | Jones |
| Nut   | NULL  |
| Bolt  | Jones |
| Bolt  | Blake |
| Bolt  | NULL  |
| Bolt  | NULL  |
| Screw | NULL  |
| Screw | NULL  |
| Cam   | NULL  |
| Cam   | NULL  |
| Cog   | NULL  |

All products are included. Supplier names are shown only for Paris suppliers; otherwise, NULL is displayed. This shows which products are supplied by Paris suppliers and which aren't.