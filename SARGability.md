## Group 1: SARGability & Query Rewrites (Search Argumentable)

### Q1.1: Math on Filter Columns
```
SELECT 
	product_name,
	price 
FROM products
WHERE price > 100.00 / 1.10;
```

### Q2. Date Range Filtering vs. EXTRACT() / YEAR()
```
SELECT
	* 
FROM orders
WHERE order_date >= '2026-01-01' AND order_date < '2027-01-01';
```

### Q3. String Case-Insensitivity
```
SELECT * FROM employees 
WHERE department ILIKE 'ENGINEERING';
```

### Q4. Implicit Data Type Conversion (employee_id were stored as VARCHAR)
```
SELECT * FROM emloyees 
WHERE employee_id = '101'
```

### Q5. Replacing OR with UNION ALL
```
SELECT * FROM orders
WHERE employee_id = 101
UNION ALL
SELECT * FROM orders
WHERE product_id = 201 AND employee_id != 101;
```
