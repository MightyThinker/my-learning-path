# MySQL Select Command

- used to retrieve data in MySQL
- reserved keyword
- For selected columns we give column names separated by comma
```sql
-- Format
SELECT col1, col2, ..., colN FROM table_name;
```
- If we want all columns data we can use wild card `*`
```sql
SELECT * FROM table_name;
```
- We can give alis or place holder to column name and it is only for viewing purpose and not chnaging any schema, we can use `AS` keyword.
```sql
SELECT col1 AS alias1, col2 AS alias2 FROM table_name;
```
- We will study different clauses or functions we can use to get our data or filter data.

- Order of Command Execution:
    1. **FROM:**
        - identify source table or views
        - perform `JOIN` operation with `ON`condition fron working data set.
    2. **WHERE:**
        - filter data based on conditions
        - it runs before `GROUP BY`
    3. **GROUP BY:**
        - groups remaining rows based on 1 or more columns or expressions
        - it runs before `HAVING`
    4. **HAVING:**
        - filter groups based on conditions
        - it runs after `GROUP BY`
    5. **SELECT:**
        - choose which columns or expressions to includ in the final result
    6. **DISTINCT:**
        - remove duplicate rows if specified columns from the result set
    7. **ORDER BY:**
        - sort remaining rows in ascending [ASC] or descending [DESC] order
    8. **LIMIT:**
        - limit the number of rows returned

## Sample Code
```sql
-- Create Table employee
CREATE TABLE employee (
    emp_id INT AUTO_INCREMENT,
    fname VARCHAR(40) NOT NULL,
    lname VARCHAR(20) NOT NULL,
    email VARCHAR(50) UNIQUE,
    department VARCHAR(50),
    salary DECIMAL(10,2) CHECK (salary > 0.0),
    emp_status ENUM('Active', 'Inactive') DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- tell sql to put current timestamp on updation of row
    CONSTRAINT emp_id_pk PRIMARY KEY (emp_id)
);

-- Inserting Data in Employee
INSERT INTO employee (fname, lname, email, department, salary, emp_status)
VALUES ('John', 'Doe', 'john.doe@example.com', 'IT', 50000.00, 'Active'),
('Jane', 'Doe', 'jane.doe@example.com', 'HR', 60000.00, 'Active'),
('Bob', 'Smith', 'bob.smith@example.com', 'IT', 55000.00, 'Inactive'),
('Alice', 'Johnson', 'alice.j@example.com', 'Accounts', 52000.00, 'Active');

-- Select Everything from Employee Table
SELECT * FROM employee;

-- Display full name for employe as FULL_NAME
SELECT fname || ' ' || lname AS FULL_NAME FROM employee;
SELECT CONCAT(fname, ' ', lname) AS FULL_NAME FROM employee;

-- Display department available and no duplicates
SELECT DISTINCT department FROM employee;

-- Get IT department employees with salaey in reverse sorted order
SELECT * FROM employee WHERE department = 'IT' ORDER BY salary DESC;

-- Show employee id, name, salary with 10% increemnt in the salary as INCREMENTED_SALARY
SELECT emp_id, fname, lname, salary, salary * 1.10 AS INCREMENTED_SALARY FROM employee;

-- Get employees whose salary is greater than average salary
SELECT * FROM employee WHERE salary > (SELECT AVG(salary) FROM employee);
```