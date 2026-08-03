# Group 3: Join Optimization & Execution Plans
---
## Join mechanics dictate how memory and CPU are allocated during query processing.
---
### 3.1: Understanding Join Algorithms
**Nested Loop Join: Best for small outer datasets with indexed inner table lookups.**
*Hash Join: Used for large, unsorted datasets; builds a temporary hash table in memory.*
*Merge Join: Fastest join algorithm, but requires both inputs to be pre-sorted on the join keys.*

```
SELECT e.first_name, o.order_id 
FROM Employees e 
JOIN Orders o ON e.employee_id = o.employee_id
AND o.status = 'COMPLETED';
```
---

### Putting a filter on the right table in the WHERE clause turns a LEFT JOIN into an INNER JOIN. Filtering during the join stage reduces the row count early.

### 3.2: Foreign Key Indexing
#### Foreign Keys do not create indexes automatically in PostgreSQL. Unindexed FKs cause slow join performance and locking issues during parent table deletions.

---

### 3.3: Replacing Correlated Subqueries with Joins
```
SELECT p.product_name, COUNT(o.order_id) AS total_orders
FROM Products p
LEFT JOIN Orders o ON p.product_id = o.product_id
GROUP BY p.product_id, p.product_name;
```
---

#### Correlated subqueries execute once for every outer table row (RBAR - Row By Agonizing Row). Joins process entire sets in memory at once.

### 3.4: Avoiding Unnecessary DISTINCT
```
SELECT e.employee_id, e.first_name 
FROM Employees e 
WHERE EXISTS (SELECT 1 FROM Orders o WHERE o.employee_id = e.employee_id);
```

### DISTINCT triggers an expensive memory sort or hash operation. EXISTS short-circuits as soon as the first matching order row is found.
