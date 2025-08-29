# Complete SQL Tutorial

## Table of Contents
- [What is SQL?](#what-is-sql)
- [Basic Concepts](#basic-concepts)
- [Data Types](#data-types)
- [Creating Tables](#creating-tables)
- [Basic Queries](#basic-queries)
- [Filtering Data](#filtering-data)
- [Sorting and Limiting](#sorting-and-limiting)
- [Modifying Data](#modifying-data)
- [Joins](#joins)
- [Aggregate Functions](#aggregate-functions)
- [Advanced Features](#advanced-features)
- [Best Practices](#best-practices)

## What is SQL?

SQL (Structured Query Language) is the standard language for managing relational databases. It allows you to create, read, update, and delete data, as well as manage database structure.

## Basic Concepts

- **Database**: A collection of related tables
- **Table**: A collection of rows and columns (like a spreadsheet)
- **Row/Record**: A single entry in a table
- **Column/Field**: A data attribute in a table
- **Primary Key**: A unique identifier for each row
- **Foreign Key**: A reference to a primary key in another table

## Data Types

### Common SQL Data Types
- `INT` or `INTEGER` - Whole numbers
- `VARCHAR(n)` - Variable-length strings (max n characters)
- `CHAR(n)` - Fixed-length strings
- `DATE` - Date values (YYYY-MM-DD)
- `TIME` - Time values (HH:MM:SS)
- `DATETIME` or `TIMESTAMP` - Date and time combined
- `DECIMAL(p,s)` - Decimal numbers (p digits, s after decimal)
- `BOOLEAN` - True/false values
- `TEXT` - Large text fields

## Creating Tables

### Basic Table Creation
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    hire_date DATE,
    salary DECIMAL(10,2)
);
```

### Adding Constraints
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATE DEFAULT CURRENT_DATE,
    total_amount DECIMAL(10,2) CHECK (total_amount > 0),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

## Basic Queries

### SELECT Statement
```sql
-- Select all columns
SELECT * FROM employees;

-- Select specific columns
SELECT first_name, last_name, salary FROM employees;

-- Select with alias
SELECT first_name AS "First Name", salary AS "Annual Salary" FROM employees;
```

### DISTINCT Values
```sql
SELECT DISTINCT department FROM employees;
```

## Filtering Data

### WHERE Clause
```sql
-- Basic conditions
SELECT * FROM employees WHERE salary > 50000;
SELECT * FROM employees WHERE department = 'Sales';

-- Multiple conditions
SELECT * FROM employees WHERE salary > 50000 AND department = 'Engineering';
SELECT * FROM employees WHERE department = 'Sales' OR department = 'Marketing';

-- Pattern matching
SELECT * FROM employees WHERE first_name LIKE 'J%';  -- Starts with 'J'
SELECT * FROM employees WHERE email LIKE '%@gmail.com';  -- Ends with '@gmail.com'

-- Range queries
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 80000;
SELECT * FROM employees WHERE hire_date BETWEEN '2020-01-01' AND '2023-12-31';

-- NULL checks
SELECT * FROM employees WHERE phone_number IS NULL;
SELECT * FROM employees WHERE phone_number IS NOT NULL;

-- IN operator
SELECT * FROM employees WHERE department IN ('Sales', 'Marketing', 'HR');
```

## Sorting and Limiting

### ORDER BY
```sql
-- Sort ascending (default)
SELECT * FROM employees ORDER BY last_name;

-- Sort descending
SELECT * FROM employees ORDER BY salary DESC;

-- Multiple sort criteria
SELECT * FROM employees ORDER BY department ASC, salary DESC;
```

### LIMIT
```sql
-- Get first 10 records
SELECT * FROM employees LIMIT 10;

-- Skip first 10, get next 10 (pagination)
SELECT * FROM employees LIMIT 10 OFFSET 10;
```

## Modifying Data

### INSERT - Adding Data
```sql
-- Insert single record
INSERT INTO employees (first_name, last_name, email, hire_date, salary)
VALUES ('John', 'Doe', 'john.doe@company.com', '2023-01-15', 65000);

-- Insert multiple records
INSERT INTO employees (first_name, last_name, email, salary) VALUES
('Jane', 'Smith', 'jane.smith@company.com', 70000),
('Bob', 'Johnson', 'bob.johnson@company.com', 55000);
```

### UPDATE - Modifying Data
```sql
-- Update single record
UPDATE employees 
SET salary = 75000 
WHERE id = 1;

-- Update multiple records
UPDATE employees 
SET department = 'Engineering' 
WHERE department = 'IT';

-- Update with calculations
UPDATE employees 
SET salary = salary * 1.05 
WHERE performance_rating = 'Excellent';
```

### DELETE - Removing Data
```sql
-- Delete specific records
DELETE FROM employees WHERE id = 1;

-- Delete with conditions
DELETE FROM employees WHERE hire_date < '2020-01-01';

-- Delete all records (be careful!)
DELETE FROM employees;
```

## Joins

### INNER JOIN
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

### LEFT JOIN
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

### RIGHT JOIN
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```

### FULL OUTER JOIN
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

## Aggregate Functions

### Basic Aggregates
```sql
-- Count records
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department) FROM employees;

-- Sum values
SELECT SUM(salary) FROM employees;

-- Average
SELECT AVG(salary) FROM employees;

-- Min/Max
SELECT MIN(salary), MAX(salary) FROM employees;
```

### GROUP BY
```sql
-- Group by single column
SELECT department, COUNT(*), AVG(salary)
FROM employees
GROUP BY department;

-- Group by multiple columns
SELECT department, hire_year, COUNT(*)
FROM employees
GROUP BY department, YEAR(hire_date);
```

### HAVING
```sql
-- Filter groups (use HAVING instead of WHERE for aggregates)
SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

## Advanced Features

### Subqueries
```sql
-- Subquery in WHERE
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Subquery in FROM
SELECT dept_avg.department, dept_avg.avg_salary
FROM (
    SELECT department, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department
) dept_avg
WHERE dept_avg.avg_salary > 65000;
```

### Common Table Expressions (CTE)
```sql
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 80000
),
departments_summary AS (
    SELECT department, COUNT(*) as count, AVG(salary) as avg_sal
    FROM high_earners
    GROUP BY department
)
SELECT * FROM departments_summary WHERE avg_sal > 90000;
```

### Window Functions
```sql
-- Ranking
SELECT first_name, last_name, salary,
       RANK() OVER (ORDER BY salary DESC) as salary_rank
FROM employees;

-- Row number within groups
SELECT first_name, last_name, department, salary,
       ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank
FROM employees;
```

### CASE Statements
```sql
SELECT first_name, last_name, salary,
    CASE 
        WHEN salary > 80000 THEN 'High'
        WHEN salary > 50000 THEN 'Medium'
        ELSE 'Low'
    END as salary_category
FROM employees;
```

## Best Practices

### Query Optimization
- Use indexes on frequently queried columns
- Avoid `SELECT *` in production code
- Use LIMIT when you don't need all results
- Write readable queries with proper indentation
- Use table aliases for cleaner code

### Security
- Always use parameterized queries to prevent SQL injection
- Grant minimum necessary privileges
- Regularly backup your databases

### Naming Conventions
- Use lowercase with underscores for table and column names
- Use singular nouns for table names (user, not users)
- Use descriptive names
- Be consistent across your schema

### Example Database Schema
```sql
-- Create a sample database
CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    category VARCHAR(50),
    stock_quantity INT DEFAULT 0
);

CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATE DEFAULT CURRENT_DATE,
    total_amount DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'pending',
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

CREATE TABLE order_items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

## Common SQL Commands Reference

### Database Management
```sql
-- Create database
CREATE DATABASE company_db;

-- Use database
USE company_db;

-- Drop database
DROP DATABASE company_db;
```

### Table Management
```sql
-- Show tables
SHOW TABLES;

-- Describe table structure
DESCRIBE employees;

-- Add column
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Modify column
ALTER TABLE employees MODIFY COLUMN phone VARCHAR(25);

-- Drop column
ALTER TABLE employees DROP COLUMN phone;

-- Drop table
DROP TABLE employees;
```

### Indexes
```sql
-- Create index
CREATE INDEX idx_employee_email ON employees(email);

-- Create unique index
CREATE UNIQUE INDEX idx_employee_id ON employees(id);

-- Drop index
DROP INDEX idx_employee_email ON employees;
```

This tutorial covers the essential SQL concepts and commands you'll need for most database operations. Practice with sample data to reinforce these concepts!
