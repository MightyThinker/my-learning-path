# MySQL: SELECT, ALIAS, DISTINCT

## SELECT
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

## ALIAS
- temporary names assigned to database tables, columns or expressions to make them readable or manageable.
- it is used in `JOIN` operation by giving table alias, subqueries, `CASE` expression, functions, etc.
- we use `AS` keyword to alias and some DB like Oracle do not support `AS` keyword.
- using `AS` is optional in case of table alias in `JOIN`.
```sql
-- Column alias
SELECT col1 AS alias1, col2 AS alias2 FROM table_name;

-- Table alias
SELECT t1.column_name FROM table_name t1;

-- Join Operation alias
SELECT t1.column_name FROM table_name t1 JOIN table_name t2 ON t1.id = t2.id;

-- Subquery alias
SELECT (SELECT column_name FROM table_name) AS alias FROM table_name;

-- CASE expression alias
SELECT 
    CASE
        WHEN condition THEN result
        ELSE result
    END AS alias
FROM table_name;
```
- Aliasing can shorten or rename columns or expressions to make them more readable or manageable.

## DISTINCT
- it is used to remove duplicates from result set.
- it is used before *column_name* or *expression* in `SELECT`.
- it get applied to combination of entire row of selected columns and not individual columns.
```sql
SELECT DISTINCT col1, col2, ..., colN FROM table_name;
```
- `NULL` values are treated unique by `DISTINCT` and if multiple `NULL` values are there then result set will have only 1 `NULL`.
- if database is large and indexing is not available then we need optimization as there will be performance issues.


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

-- Display full name for employe as FULL_NAME alias
SELECT fname || ' ' || lname AS FULL_NAME FROM employee;
SELECT CONCAT(fname, ' ', lname) AS FULL_NAME FROM employee;

-- Display unique department available
SELECT DISTINCT department FROM employee;

-- Get IT department employees with salaey in reverse sorted order
SELECT * FROM employee WHERE department = 'IT' ORDER BY salary DESC;

-- Show employee id, name, salary with 10% increemnt in the salary as INCREMENTED_SALARY
SELECT emp_id, fname, lname, salary, salary * 1.10 AS INCREMENTED_SALARY FROM employee;

-- Get employees whose salary is greater than average salary
SELECT * FROM employee WHERE salary > (SELECT AVG(salary) FROM employee);

-- case example
SELECT 
    emp_id,
    fname,
    lname,
    salary,
    CASE
        WHEN salary > (SELECT AVG(salary) FROM employee) THEN 'High'
        WHEN salary < (SELECT AVG(salary) FROM employee) THEN 'Low'
        ELSE 'Medium'
    END AS salary_level
FROM employee;

-- Subquery alias
SELECT t.avg_sal FROM (SELECT AVG(salary) AS avg_sal FROM employee) t;
```