## 1. Categories
```SQL
CREATE TABLE Categories (
    category_id INT PRIMARY KEY,
    category_name VARCHAR(50) NOT NULL
);
```

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
``
