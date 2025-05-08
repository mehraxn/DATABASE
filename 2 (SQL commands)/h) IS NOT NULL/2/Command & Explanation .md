# SQL Scenarios with Sample Data Tables

## Scenario 1: Employee Promotion Eligibility

### Sample Data Tables

**Table: employees**

| employee_id | name | manager_id | department_id | promotion_date |
|-------------|------|------------|---------------|----------------|
| 1 | John Doe | 5 | 1 | 2023-06-15 |
| 2 | Jane Smith | 5 | 1 | NULL |
| 3 | Robert Brown | 6 | 2 | NULL |
| 4 | Lisa Wong | NULL | 3 | NULL |
| 5 | Michael Chen | NULL | 1 | 2022-04-10 |
| 6 | Sarah Johnson | 5 | 2 | 2023-01-22 |
| 7 | James Wilson | 6 | NULL | NULL |
| 8 | Emily Davis | 5 | 3 | NULL |

**Table: departments**

| department_id | name |
|---------------|------|
| 1 | Engineering |
| 2 | Marketing |
| 3 | Sales |
| 4 | Finance |

### SQL Command
```sql
SELECT e.employee_id, e.name, d.name AS department_name 
FROM employees e 
JOIN departments d ON e.department_id = d.department_id 
WHERE e.promotion_date IS NULL 
AND e.manager_id IS NOT NULL 
AND e.department_id IS NOT NULL;
```

### Description
This query finds all employees who are eligible for promotion based on three criteria:
1. They have no promotion date yet (`promotion_date IS NULL`)
2. They have a manager assigned (`manager_id IS NOT NULL`)
3. They belong to a department (`department_id IS NOT NULL`)

### Explanation
The query joins the employees table with the departments table to get the department name for each eligible employee. The WHERE clause filters for employees meeting all three conditions. From our sample data, the query would return:

**Result**

| employee_id | name | department_name |
|-------------|------|-----------------|
| 2 | Jane Smith | Engineering |
| 3 | Robert Brown | Marketing |
| 8 | Emily Davis | Sales |

These employees have managers, belong to departments, and haven't been promoted yet.

## Scenario 2: Product Supply Gap Detection

### Sample Data Tables

**Table: products**

| product_id | name | supplier_id |
|------------|------|-------------|
| 101 | Laptop Pro | 1 |
| 102 | Wireless Mouse | 1 |
| 103 | Keyboard Elite | 2 |
| 104 | Monitor 24" | NULL |
| 105 | USB-C Cable | 3 |
| 106 | Headphones | 4 |
| 107 | Webcam HD | 5 |
| 108 | Desk Lamp | NULL |

**Table: suppliers**

| supplier_id | name | contact_email |
|-------------|------|---------------|
| 1 | TechGear Inc. | sales@techgear.com |
| 2 | ElitePeripherals | NULL |
| 3 | CableSpecialists | info@cablespec.com |
| 4 | AudioMax | NULL |
| 5 | VisualPro | contact@visualpro.com |

### SQL Command
```sql
SELECT p.product_id, p.name 
FROM products p 
LEFT JOIN suppliers s ON p.supplier_id = s.supplier_id 
WHERE p.supplier_id IS NULL OR s.contact_email IS NULL;
```

### Description
This query lists all products that either:
1. Have no supplier assigned (`supplier_id IS NULL`)
2. Have a supplier, but the supplier has no contact email available

### Explanation
The query uses a LEFT JOIN to include all products regardless of whether they have a matching supplier. The WHERE clause filters for products with no supplier or products whose supplier has no contact email. From our sample data, the query would return:

**Result**

| product_id | name |
|------------|------|
| 103 | Keyboard Elite |
| 104 | Monitor 24" |
| 106 | Headphones |
| 108 | Desk Lamp |

These products either have no supplier (Monitor 24", Desk Lamp) or have suppliers with missing contact emails (Keyboard Elite from ElitePeripherals, Headphones from AudioMax).

## Scenario 3: Students Without Assigned Advisors

### Sample Data Tables

**Table: students**

| student_id | name | advisor_id |
|------------|------|------------|
| 1001 | Alex Johnson | 501 |
| 1002 | Maria Garcia | 502 |
| 1003 | Tyler Smith | NULL |
| 1004 | Zoe Williams | 503 |
| 1005 | Ryan Thompson | 504 |
| 1006 | Emma Brown | 505 |
| 1007 | Jason Lee | 502 |
| 1008 | Sofia Martinez | 506 |

**Table: advisors**

| advisor_id | name | active_flag |
|------------|------|-------------|
| 501 | Dr. Anderson | 1 |
| 502 | Prof. Williams | 0 |
| 503 | Dr. Chen | 1 |
| 504 | Prof. Rodriguez | NULL |
| 505 | Dr. Johnson | 1 |
| 506 | Prof. Wilson | 0 |

### SQL Command
```sql
SELECT s.student_id, s.name 
FROM students s 
LEFT JOIN advisors a ON s.advisor_id = a.advisor_id 
WHERE s.advisor_id IS NULL OR a.active_flag IS NULL OR a.active_flag = 0;
```

### Description
This query identifies students who either:
1. Have no advisor assigned at all
2. Have an advisor who is no longer active (active_flag = 0)
3. Have an advisor whose active status is unknown (active_flag IS NULL)

### Explanation
The query uses a LEFT JOIN to include all students even if they don't have a matching advisor. The WHERE clause filters for students with no advisor or with inactive/unknown status advisors. From our sample data, the query would return:

**Result**

| student_id | name |
|------------|------|
| 1002 | Maria Garcia |
| 1003 | Tyler Smith |
| 1005 | Ryan Thompson |
| 1007 | Jason Lee |
| 1008 | Sofia Martinez |

These students either have no advisor (Tyler Smith), have inactive advisors (Maria Garcia, Jason Lee, Sofia Martinez have Prof. Williams or Prof. Wilson), or have advisors with unknown status (Ryan Thompson has Prof. Rodriguez).

## Scenario 4: Orders with Missing Shipment Info

### Sample Data Tables

**Table: orders**

| order_id | customer_id | ship_date |
|----------|-------------|-----------|
| 10001 | 1 | 2025-04-10 |
| 10002 | 2 | 2025-04-12 |
| 10003 | 3 | NULL |
| 10004 | 1 | 2025-04-15 |
| 10005 | 4 | 2025-04-18 |
| 10006 | 2 | 2025-04-20 |
| 10007 | 5 | NULL |
| 10008 | 3 | 2025-05-01 |

**Table: customers**

| customer_id | name |
|-------------|------|
| 1 | John Smith |
| 2 | Laura Jones |
| 3 | Mark Wilson |
| 4 | Anna Garcia |
| 5 | Robert Chen |

**Table: shipments**

| order_id | tracking_number |
|----------|-----------------|
| 10001 | TRK123456 |
| 10002 | NULL |
| 10004 | TRK789012 |
| 10006 | TRK345678 |
| 10008 | NULL |

### SQL Command
```sql
SELECT o.order_id, c.name AS customer_name, o.ship_date 
FROM orders o 
JOIN customers c ON o.customer_id = c.customer_id 
LEFT JOIN shipments s ON o.order_id = s.order_id 
WHERE o.ship_date IS NOT NULL AND s.tracking_number IS NULL;
```

### Description
This query finds all orders that:
1. Have a ship date specified (they were supposed to be shipped)
2. Don't have a tracking number recorded in the shipments table

### Explanation
The query joins the orders table with customers to get customer names, and uses a LEFT JOIN with shipments to include orders even if they don't have a shipment record. The WHERE clause filters for orders with a ship date but no tracking number. From our sample data, the query would return:

**Result**

| order_id | customer_name | ship_date |
|----------|---------------|-----------|
| 10002 | Laura Jones | 2025-04-12 |
| 10005 | Anna Garcia | 2025-04-18 |
| 10008 | Mark Wilson | 2025-05-01 |

These orders have been marked for shipping (have a ship date) but either don't have an entry in the shipments table (order 10005) or have a NULL tracking number (orders 10002 and 10008).

## Scenario 5: Unused Coupons Report

### Sample Data Tables

**Table: coupons**

| coupon_id | code | expiration_date |
|-----------|------|-----------------|
| 1 | SPRING25 | 2025-06-30 |
| 2 | SUMMER10 | 2025-09-15 |
| 3 | FALL2025 | 2025-12-01 |
| 4 | WINTER50 | 2025-03-01 |
| 5 | WELCOME15 | 2026-01-31 |
| 6 | FLASH20 | 2025-05-15 |

**Table: redemptions**

| coupon_id | user_id | redemption_date |
|-----------|---------|-----------------|
| 1 | 101 | 2025-04-10 |
| 1 | 102 | 2025-04-15 |
| 2 | 103 | 2025-05-01 |
| 4 | 104 | 2025-02-10 |
| 6 | 105 | 2025-05-05 |

### SQL Command
```sql
SELECT c.coupon_id, c.code, c.expiration_date 
FROM coupons c 
LEFT JOIN redemptions r ON c.coupon_id = r.coupon_id 
WHERE c.expiration_date > CURRENT_DATE AND r.redemption_date IS NULL;
```

### Description
This query lists all coupons that:
1. Have not expired yet (expiration_date is in the future)
2. Have never been redeemed by any user

### Explanation
The query uses a LEFT JOIN to include all coupons regardless of whether they have redemption records. The WHERE clause filters for non-expired coupons with no redemption date. From our sample data (assuming today is May 8, 2025), the query would return:

**Result**

| coupon_id | code | expiration_date |
|-----------|------|-----------------|
| 3 | FALL2025 | 2025-12-01 |
| 5 | WELCOME15 | 2026-01-31 |

These coupons have not expired and have never been used by any customer. Coupons WINTER50 and FLASH20 are either expired or have been redeemed, while SPRING25 and SUMMER10 have been redeemed by at least one customer.