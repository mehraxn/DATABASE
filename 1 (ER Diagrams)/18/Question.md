# Strategic Placement of Temporal Attributes in ER Diagrams: The Case of Start and End Dates

## Introduction

In database design, particularly when modeling temporal data, the placement of attributes like start and end dates requires careful consideration. This article examines a common pattern found in historicized relationships, where start dates are associated with time entities while end dates are attached to relationships. Using a retail price tracking system as our case study, we'll explore the rationale behind this design decision and its implications for temporal data management.

## The Case Study: A Technology Product Store

Consider the following scenario:

> A technology product store wants to memorize the price list. Each product is marked with a barcode, brand, and model. The products are divided into different categories (cameras, smartphones, PCs)... The price list keeps track of the prices at which each product was sold over time.

In the corresponding ER diagram, we observe:
- A TIME entity with a StartDate attribute
- A PRICE_LIST relationship connecting PRODUCT and TIME
- An EndDate attribute attached to the PRICE_LIST relationship with (0,1) cardinality
- The relationship has (1,N) cardinality on both sides

## The Fundamental Question

Why is the EndDate attribute placed on the PRICE_LIST relationship rather than on the TIME entity alongside StartDate? And why does it have (0,1) cardinality?

## Analysis of Attribute Placement

### Why StartDate Belongs to the TIME Entity

1. **Intrinsic Property**: StartDate is an inherent characteristic of the TIME entity itself. It defines the beginning of a time period as a fundamental property.

2. **Universal Reference Point**: As a primary defining attribute of TIME, StartDate serves as a universal reference point for all entities related to that time period.

3. **Entity Identification**: StartDate likely contributes to identifying specific TIME instances, potentially serving as part of the primary key.

### Why EndDate Belongs to the PRICE_LIST Relationship

1. **Relationship-Specific Attribute**: The EndDate is contextual to the specific relationship between a PRODUCT and a TIME period. It indicates when a particular pricing arrangement ceased to be valid.

2. **Variable Endings**: Different products may stop being priced at a certain amount at different times, even if they started at the same time. This relationship-specific information is better modeled as an attribute of the relationship.

3. **Logical Separation**: While StartDate defines when temporal records begin in general, EndDate marks specifically when a product's price validity ends, making it conceptually tied to the pricing relationship.

4. **Historicized Relationship Pattern**: The PRICE_LIST relationship is implementing a historicized pattern where temporal boundaries define relationship validity. EndDate is part of what defines this historical relationship boundary.

## The Significance of (0,1) Cardinality for EndDate

The (0,1) cardinality associated with EndDate indicates that this attribute is optional - a price listing may or may not have an end date. This design choice supports several important aspects of temporal data management:

1. **Current Validity**: Records representing currently active prices don't have an end date yet (NULL value). This makes it easy to query for current prices by finding records where EndDate is NULL.

2. **Open vs. Closed Intervals**: The design supports both closed time intervals (with defined start and end points) and open intervals (with only a start date, extending into the indefinite future).

3. **Historical Transitions**: When a product's price changes, the system can close the previous price record by setting its EndDate, while creating a new price record with the current date as StartDate and NULL as EndDate.

## Implementation Advantages

This design approach offers several practical advantages:

### 1. Efficient Querying

```sql
-- Find current price for a product
SELECT Price
FROM PriceList
WHERE ProductID = ? AND EndDate IS NULL;

-- Find price of a product at a specific point in time
SELECT Price
FROM PriceList
WHERE ProductID = ?
AND StartDate <= '2023-06-15'
AND (EndDate IS NULL OR EndDate > '2023-06-15');
```

### 2. Maintaining Historical Integrity

When a price changes, the system only needs to:
1. Update the EndDate of the current price record
2. Insert a new record with the new price

This approach preserves the complete pricing history without data loss or modification of historical records.

### 3. Flexible Time Period Management

The (0,1) cardinality of EndDate allows for flexible handling of time periods:
- Planned future price changes can be pre-recorded with defined start and end dates
- Current prices can remain open-ended until changed
- Historical analysis can examine completed pricing periods

## Alternative Designs and Their Limitations

### Alternative 1: Both Dates on the TIME Entity

If both StartDate and EndDate were attributes of the TIME entity:
- Every time a product price changed, a new TIME record would be needed
- Multiple products changing prices on different days would result in many fragmented TIME records
- It would be harder to represent current prices with indefinite end dates

### Alternative 2: Both Dates on the Relationship

If both StartDate and EndDate were on the PRICE_LIST relationship:
- The TIME entity would lose its primary defining characteristic
- There would be redundancy if multiple products share the same time periods
- The conceptual separation between a time period's definition and its application would be lost

## Conclusion

The strategic placement of temporal attributes in ER diagrams has significant implications for database functionality, query efficiency, and conceptual clarity. The pattern of associating StartDate with a TIME entity while placing EndDate on relationships (with 0,1 cardinality) elegantly supports the tracking of historicized relationships.

This design pattern is particularly valuable in domains requiring temporal data management, such as:
- Product pricing histories
- Employee position and salary tracking
- Subscription service periods
- Contract terms and renewals
- Inventory level changes over time

By understanding the rationale behind attribute placement decisions, database designers can create more effective temporal data models that accurately capture both current states and historical changes while enabling efficient querying and maintenance.