# Comparing Correlated vs. Non-Correlated Subqueries with MAX Function

This article examines two SQL queries that use the MAX aggregate function within subqueries. While they appear similar, the presence or absence of correlation significantly changes their behavior and results. We'll analyze both queries using sample data to demonstrate how correlation affects their execution and output.

## The Queries

Let's examine the following two SQL queries:

### Query 1: Correlated Subquery with MAX
```sql
SELECT PId, SId 
FROM SP AS SPX 
WHERE Qty = (
    SELECT MAX(Qty) 
    FROM SP AS SPY 
    WHERE SPY.PId = SPX.PId
);
```

### Query 2: Non-Correlated Subquery with MAX
```sql
SELECT PId, SId 
FROM SP AS SPX 
WHERE Qty = (
    SELECT MAX(Qty) 
    FROM SP AS SPY
);
```

## The Table

To evaluate these queries, we'll use the following sample data:

**Table SP (Supplier-Parts)**
| SId | PId | Qty |
|-----|-----|-----|
| S1  | P1  | 300 |
| S2  | P1  | 300 |
| S1  | P2  | 200 |
| S2  | P2  | 400 |
| S3  | P2  | 200 |
| S4  | P4  | 300 |
| S4  | P5  | 400 |
| S1  | P3  | 400 |
| S1  | P6  | 100 |

**Table S (Suppliers) - For context**
| SId | SName       | Status | City     |
|-----|-------------|--------|----------|
| S1  | Smith       | 20     | London   |
| S2  | Jones       | 10     | Paris    |
| S3  | Blake       | 30     | Paris    |
| S4  | Clark       | 20     | London   |
| S5  | Adams       | 30     | Athens   |

**Table P (Parts) - For context**
| PId | PName   | Color  | Weight | City    |
|-----|---------|--------|--------|---------|
| P1  | Nut     | Red    | 12     | London  |
| P2  | Bolt    | Green  | 17     | Paris   |
| P3  | Screw   | Blue   | 17     | Rome    |
| P4  | Screw   | Red    | 14     | London  |
| P5  | Cam     | Blue   | 12     | Paris   |
| P6  | Cog     | Red    | 19     | London  |

## Query Analysis and Results

### Query 1: Correlated Subquery with MAX

```sql
SELECT PId, SId 
FROM SP AS SPX 
WHERE Qty = (
    SELECT MAX(Qty) 
    FROM SP AS SPY 
    WHERE SPY.PId = SPX.PId
);
```

#### Execution Process:
1. The database examines each row in the SP table (aliased as SPX)
2. For each row, it executes the subquery: `SELECT MAX(Qty) FROM SP AS SPY WHERE SPY.PId = SPX.PId`
3. The correlation condition `SPY.PId = SPX.PId` restricts the subquery to only consider rows with the same part ID
4. The subquery finds the maximum quantity for that specific part
5. The row is included in the result if its quantity equals the maximum quantity for that part

#### Result of Query 1:
| PId | SId |
|-----|-----|
| P1  | S1  |
| P1  | S2  |
| P2  | S2  |
| P3  | S1  |
| P4  | S4  |
| P5  | S4  |
| P6  | S1  |

**Explanation**: 
- For part P1, both S1 and S2 have the maximum quantity (300)
- For part P2, only S2 has the maximum quantity (400)
- For part P3, S1 has the maximum quantity (400)
- For part P4, S4 has the maximum quantity (300)
- For part P5, S4 has the maximum quantity (400)
- For part P6, S1 has the maximum quantity (100)

This query finds the supplier(s) that provide the maximum quantity of each part.

### Query 2: Non-Correlated Subquery with MAX

```sql
SELECT PId, SId 
FROM SP AS SPX 
WHERE Qty = (
    SELECT MAX(Qty) 
    FROM SP AS SPY
);
```

#### Execution Process:
1. The database executes the subquery: `SELECT MAX(Qty) FROM SP AS SPY`
2. This subquery finds the maximum quantity across ALL parts in the SP table (400)
3. The main query then selects rows where Qty equals this overall maximum

#### Result of Query 2:
| PId | SId |
|-----|-----|
| P2  | S2  |
| P5  | S4  |
| P3  | S1  |

**Explanation**: Only those rows where Qty = 400 (the maximum quantity across the entire table) are returned. This includes:
- S2 supplying part P2 with quantity 400
- S4 supplying part P5 with quantity 400
- S1 supplying part P3 with quantity 400

## Key Differences

### Correlation vs. Non-Correlation

1. **Query 1 (Correlated)**:
   - The subquery executes for each row in the outer query
   - Contains a reference to the outer query (SPX.PId)
   - The MAX function operates within the context of each part
   - Result: Returns suppliers with the maximum quantity for EACH part

2. **Query 2 (Non-Correlated)**:
   - The subquery executes once
   - No reference to the outer query
   - The MAX function operates across the entire table
   - Result: Returns only suppliers with the globally maximum quantity

### Logical Interpretation

- **Query 1** asks: "For each part, which supplier(s) provide the maximum quantity?"
- **Query 2** asks: "Which supplier-part combinations have the highest quantity across all parts?"

## Business Use Cases

### When to Use Correlated MAX (Query 1)

This query is useful when you need to find the maximum value within groups, such as:
- Identifying preferred suppliers for each part (those who can supply the most)
- Finding the top-selling product in each category
- Determining the highest-paid employee in each department
- Identifying the most experienced staff member for each project

### When to Use Non-Correlated MAX (Query 2)

This query is useful when you need to find global maximum values, such as:
- Identifying the overall highest inventory items
- Finding transactions with the maximum dollar amount
- Determining which products have the highest overall sales
- Identifying peak capacity utilization instances

## Performance Considerations

1. **Query 1 (Correlated)**:
   - Less efficient as the subquery executes once per row
   - More complex execution plan with repeated calculations
   - Often represents more common business requirements
   - Optimization requires indexes on both SP.PId and SP.Qty

2. **Query 2 (Non-Correlated)**:
   - More efficient as the subquery executes only once
   - Simpler execution plan
   - Less commonly represents business needs
   - Easier for the database optimizer to handle

## Alternative Implementations

### Alternative for Query 1 (Correlated)
```sql
-- Using window functions
SELECT PId, SId
FROM (
    SELECT SP.*, MAX(Qty) OVER (PARTITION BY PId) AS MaxQty
    FROM SP
) AS SPRanked
WHERE Qty = MaxQty;

-- Using JOIN
SELECT SP1.PId, SP1.SId
FROM SP AS SP1
JOIN (
    SELECT PId, MAX(Qty) AS MaxQty
    FROM SP
    GROUP BY PId
) AS SP2 ON SP1.PId = SP2.PId AND SP1.Qty = SP2.MaxQty;
```

### Alternative for Query 2 (Non-Correlated)
```sql
-- Using a simple subquery
SELECT PId, SId
FROM SP
WHERE Qty = (SELECT MAX(Qty) FROM SP);

-- Using a CTE
WITH MaxQty AS (
    SELECT MAX(Qty) AS MaxQty
    FROM SP
)
SELECT SP.PId, SP.SId
FROM SP, MaxQty
WHERE SP.Qty = MaxQty.MaxQty;
```

## Conclusion

The correlation condition `SPY.PId = SPX.PId` fundamentally changes the behavior of the MAX function, transforming it from a global maximum to a partitioned maximum. This shifts the query's purpose from finding globally maximum quantities to finding the maximum quantities within each part category.

The choice between correlated and non-correlated subqueries with aggregate functions depends on whether you need group-level insights (correlated) or global insights (non-correlated). Understanding this distinction is crucial for writing SQL queries that correctly address business requirements.

In most real-world supply chain and inventory management scenarios, the correlated version (Query 1) is more useful as it provides insights into the optimal supplier for each specific part, rather than just identifying which parts have the highest overall quantities.