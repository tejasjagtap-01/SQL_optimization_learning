## 1. Categories
```SQL
CREATE TABLE Categories (
    category_id INT PRIMARY KEY,
    category_name VARCHAR(50) NOT NULL
);
```
### Insert Values into categories
```
INSERT INTO Categories VALUES 
(1, 'Electronics'), 
(2, 'Books'), 
(3, 'Clothing');
```

---

## 2. Employees
```SQL
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE
);
```
### Insert Values in Employees
```
INSERT INTO Employees VALUES 
(101, 'John', 'Doe', 'ENGINEERING', 75000.00, '2022-01-15'),
(102, 'Jane', 'Smith', 'Engineering', 85000.00, '2021-06-01'),
(103, 'Rahul', 'Kumar', 'Sales', 50000.00, '2023-03-10'),
(104, 'Priya', 'Sharma', 'SALES', 62000.00, '2020-11-20'),
(105, 'Amit', 'Patel', 'Engineering', 95000.00, '2019-08-05');
```

---

## 3. Products
```SQL
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category_id INT,
    price DECIMAL(10,2),
    stock_quantity INT,
    FOREIGN KEY (category_id) REFERENCES Categories(category_id)
);
```
### Insert Values into Products
```
INSERT INTO Products VALUES 
(201, 'Laptop', 1, 1200.00, 50),
(202, 'Smartphone', 1, 800.00, 150),
(203, 'SQL Performance Tuning Book', 2, 45.00, 200),
(204, 'Wireless Headphones', 1, 150.00, 80),
(205, 'Cotton T-Shirt', 3, 20.00, 500);
```

---

## 4. Orders
``` SQL
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    employee_id INT,
    product_id INT,
    order_date DATE,
    quantity INT,
    status VARCHAR(20),
    FOREIGN KEY (employee_id) REFERENCES Employees(employee_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);
```
### Insert Values into Orders
```
INSERT INTO Orders VALUES 
(1, 101, 201, '2026-01-10', 1, 'COMPLETED'),
(2, 102, 203, '2026-02-14', 2, 'COMPLETED'),
(3, 101, 204, '2026-03-01', 1, 'CANCELLED'),
(4, 104, 202, '2026-05-18', 1, 'PENDING'),
(5, 105, 201, '2026-06-22', 2, 'COMPLETED'),
(6, 103, 205, '2026-07-01', 5, 'COMPLETED');
```
