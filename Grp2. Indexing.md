## Group 2: Indexing & Storage Engine Mechanics
---
### INDEX syntax:
```
CREATE INDEX idx_orders_covering ON Orders (status) INCLUDE (order_date, quantity);
```
## 2.1: Composite Index Column Order (Leftmost Prefix Rule)
### Index Definition: CREATE INDEX idx_emp_dept_sal ON Employees(department, salary)
### Query A: WHERE department = 'Engineering' AND salary > 80000; \ Index Seek (Uses Index)
### Query B: WHERE salary > 80000; Index Scan / Table Scan (Fails Index Seek)
### Why: "Composite indexes are sorted left-to-right. 
### Filtering on trailing columns without specifying the leading column violates the Leftmost Prefix Rule."
### A Partial Index only indexes rows matching the predicate, keeping index size small, reducing memory footprint, and speeding up writes. */

## 2.2: Partial Indexes (Filtered Index)
### A Partial Index only indexes rows matching the predicate, keeping index size small, reducing memory footprint, and speeding up writes

## 2.3: The High Cost of SELECT *
### Using SELECT * forces the engine to read every column from disk, prevents the use of Covering Indexes, and wastes memory buffers during sorting or joins.

## 2.4: Over-Indexing Trade-Offs
### Every INSERT, UPDATE, or DELETE requires synchronous updating of all associated indexes. Over-indexing creates write latency and index fragmentation.
