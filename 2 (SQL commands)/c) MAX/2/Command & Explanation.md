# Comparing MAX Queries with Cartesian Products: Filtered vs. Unfiltered Approaches

This article examines two SQL queries that use the MAX aggregate function in conjunction with Cartesian products. While they may appear similar, the presence or absence of a join condition significantly changes their behavior and results. We'll analyze both queries using sample data to demonstrate the impact of filtering a Cartesian product.

## The Queries

Let's examine the following two SQL queries:

### Query 1: Filtered Cartesian Product
```sql
SELECT MAX(Qty) 
FROM SP AS SPY, SP AS SPX 
WHERE SPY.PId = SPX.PId;
```

### Query 2: Unfiltered Cartesian Product
```sql
SELECT MAX(Qty) 
FROM SP AS SPY, SP AS SPX;
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

## Query Analysis and Results

### Query 1: Filtered Cartesian Product

```sql
SELECT MAX(Qty) 
FROM SP AS SPY, SP AS SPX 
WHERE SPY.PId = SPX.PId;
```

#### Execution Process:
1. The database creates a Cartesian product of the SP table with itself (each row combined with every other row)
2. The WHERE condition `SPY.PId = SPX.PId` filters this product to only include row combinations where both rows have the same part ID
3. The MAX function then finds the maximum Qty value among all remaining rows in this filtered result set

#### Understanding the Data Generation:
When we join SP with itself and filter by matching PId:
- P1 rows join with P1 rows (2 × 2 = 4 combinations)
- P2 rows join with P2 rows (3 × 3 = 9 combinations) 
- P3 rows join with P3 rows (1 × 1 = 1 combination)
- Etc.

The result set before applying MAX includes all these combinations, with each combination containing Qty values from both tables.

#### Result of Query 1:
```
400
```

**Explanation**: The maximum Qty value across all the filtered combinations is 400, which appears in several combinations:
- When the row (S2, P2, 400) is joined with any P2 row
- When the row (S4, P5, 400) is joined with the other P5 row
- When the row (S1, P3, 400) is joined with the other P3 row

The MAX function returns the highest Qty value from the filtered Cartesian product, which is 400.

### Query 2: Unfiltered Cartesian Product

```sql
SELECT MAX(Qty) 
FROM SP AS SPY, SP AS SPX;
```

#### Execution Process:
1. The database creates a Cartesian product of the SP table with itself (each row combined with every other row)
2. There is no filtering condition, so ALL combinations are included
3. The MAX function finds the maximum Qty value among all rows in this complete Cartesian product

#### Understanding the Data Generation:
When we join SP with itself without filtering:
- Each of the 9 rows combines with all 9 rows, creating 9 × 9 = 81 combinations

The result set before applying MAX includes all these combinations, with each combination containing Qty values from both tables.

#### Result of Query 2:
```
400
```

**Explanation**: The maximum Qty value across all possible combinations is still 400. Despite having many more combinations in the unfiltered Cartesian product, the maximum value remains 400 since no higher Qty value exists in the table.

## Key Differences

### Filtered vs. Unfiltered Cartesian Products

1. **Query 1 (Filtered)**:
   - Includes only combinations where SPY.PId = SPX.PId
   - Fewer total combinations (only rows with matching part IDs)
   - Result set is smaller before applying MAX
   - The MAX function only considers Qty values from combinations with matching PIds

2. **Query 2 (Unfiltered)**:
   - Includes all possible combinations of rows
   - Many more total combinations (each row paired with every other row)
   - Result set is much larger before applying MAX
   - The MAX function considers Qty values from all possible combinations

### Data Processing Volume

- **Query 1** processes fewer combinations due to filtering
- **Query 2** processes all possible combinations (quadratic growth with table size)

### Final Result

Despite the significant differences in execution, both queries return the same result (400) for this specific dataset because:
1. The maximum Qty value in the entire table is 400
2. Filtering combinations by PId doesn't change the maximum value

## Performance Implications

1. **Query 1 (Filtered)**:
   - More efficient due to reduced data processing
   - Processes fewer combinations
   - Less memory usage
   - Faster execution for large tables

2. **Query 2 (Unfiltered)**:
   - Very inefficient due to processing all combinations
   - Processes every possible combination (n²)
   - Higher memory usage
   - Significantly slower for large tables
   - Potential to cause database performance issues

## When Results Would Differ

While these queries return the same value for our sample data, they could return different results in scenarios such as:

1. **If Qty was calculated rather than stored**:
   ```sql
   SELECT MAX(SPY.Qty + SPX.Qty) 
   FROM SP AS SPY, SP AS SPX 
   WHERE SPY.PId = SPX.PId;
   
   SELECT MAX(SPY.Qty + SPX.Qty) 
   FROM SP AS SPY, SP AS SPX;
   ```
   The filtered version would find the maximum sum within matching part groups, while the unfiltered version would find the maximum sum across any two rows.

2. **If using columns from both tables**:
   ```sql
   SELECT MAX(SPY.Qty * SPX.Qty) 
   FROM SP AS SPY, SP AS SPX 
   WHERE SPY.PId = SPX.PId;
   
   SELECT MAX(SPY.Qty * SPX.Qty) 
   FROM SP AS SPY, SP AS SPX;
   ```
   The results would likely differ as the highest product would come from different combinations.

## Alternative Implementations

For finding the maximum quantity, simpler and more efficient queries would be:

```sql
-- Simple query for maximum quantity
SELECT MAX(Qty) FROM SP;

-- Maximum quantity by part
SELECT PId, MAX(Qty) 
FROM SP
GROUP BY PId;
```

## Practical Implications and Use Cases

### Query 1 (Filtered)
Might be used when:
- Comparing values within the same group (though GROUP BY is usually better)
- Finding maximum relationships between related records
- Self-referential analysis within categories

### Query 2 (Unfiltered)
Generally should be avoided because:
- It creates a Cartesian product without filtering
- Extremely inefficient for anything but tiny tables
- Rarely represents a real business need
- Simple MAX or aggregates are almost always better

## Conclusion

While both queries return the same result (400) for this dataset, they represent fundamentally different approaches to data processing:

1. **Query 1** filters combinations by part ID before finding the maximum
2. **Query 2** considers all possible combinations without filtering

The filtered approach is significantly more efficient and represents a more typical business need (finding maximums within categories). However, both queries are suboptimal compared to simpler alternatives like `SELECT MAX(Qty) FROM SP` or using GROUP BY.

This comparison illustrates the importance of understanding how Cartesian products work in SQL and why filtering these products is critical for performance. It also demonstrates why careful query design is essential - even when two different queries return the same result, their performance characteristics can be dramatically different.