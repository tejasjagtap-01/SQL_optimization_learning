# Group 4: Aggregations, Memory & Lock Isolation
---
### 4.1: Groupings and locks consume RAM (work_mem) and can cause blocking/deadlocks under concurrent traffic.

#### Optimized Query
```
SELECT department, ROUND(AVG(salary))
FROM Employees 
WHERE salary > 50000 
GROUP BY department 
HAVING AVG(salary) > 65000;
```

*WHERE filters raw rows before grouping, reducing the volume of data stored in aggregation memory.*

### 4.2: Window Functions vs. Self-Joins
#### Q. Find the most expensive product in each category.
```
WITH RankedProducts AS (
    SELECT product_name, category_id, price,
           DENSE_RANK() OVER (PARTITION BY category_id ORDER BY price DESC) as rnk
    FROM Products
)
SELECT product_name, category_id, price 
FROM RankedProducts 
WHERE rnk = 1;
```

*Window functions evaluate partitions in a single scan, avoiding expensive self-joins on the same table.*

### 4.3: Shared vs. Exclusive Locks
*Shared Lock (SELECT): Allows multiple concurrent reads, blocks writes.
Exclusive Lock (INSERT/UPDATE/DELETE): Blocks all other reads and writes on the locked row/page.*

### 4.4: Deadlock Prevention
*Deadlocks happen when Transaction 1 holds Resource A and waits for B, while Transaction 2 holds Resource B and waits for A. 
Prevention: Always access tables in the exact same deterministic order across application transactions.*

### 4.5: Transaction Isolation Levels
*READ COMMITTED (Postgres Default): Prevents dirty reads. Reads only committed data.*
*REPEATABLE READ: Guarantees that data read during a transaction will remain identical across re-reads.*

#### SERIALIZABLE: Highest isolation level; prevents phantom reads by emulating serial transaction execution.*/

### 4.6: Database Statistics & Out-of-Date Plans
*Why: If the query optimizer makes bad plan choices (e.g., picking a scan instead of an index seek), stale statistics are often responsible. Running ANALYZE TableName; updates table metrics for the optimizer*


### Q1. Find all employees earning a 10% bonus whose total compensation (salary * 1.10) exceeds $80,000.
```
SELECT
employee_id,
first_name,
salary
FROM employees
WHERE salary > 80000.00 / 1.10
```

### Q2. Find all orders placed between January 1, 2026, and March 31, 2026.
```
SELECT 
	order_id,
	order_date,
	status
FROM orders
WHERE order_date >= '2026-01-01' AND order_date < '2026-04-01';
```

### Q3. Find the total quantity ordered for each product.
Wrong Query
```
SELECT
	p.product_name,
	COUNT(o.quantity) OVER(PARTITION BY p.product_id) 
FROM products p
LEFT JOIN orders o 
ON o.product_id = p.product_id
```

### Correct Query
```
SELECT 
    p.product_name,
    COALESCE(SUM(o.quantity), 0) AS total_quantity
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id
GROUP BY p.product_id, p.product_name;
```
```
EXPLAIN ANALYZE
SELECT order_id, order_date, status
FROM orders
WHERE order_date >= '2026-01-01' AND order_date < '2026-04-01';
```
