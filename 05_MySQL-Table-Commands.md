# MySQL Table Commands: CREATE, ALTER, DROP, INSERT

1. **CREATE:**
    - used to create table
    - reserved keyword
    ```sql
    CREATE TABLE table_name (
        column1_name column1_type constraint1,
        column2_name column2_type constraint2,
        column3_name column3_type constraint3,
        ...
        table_constraints
    );
    ```
> **NOTE:**
>   - contraints are rules applied on columns.
>   - *AUTO_INCREMENT* is a property and not a constraint.
>   - `check` constraint is written inside wrapper `()`.
>   - wrapper also known as expression evaluator is only used in case of expression and not in case of value.

> **Best Practices:**
>   - We should have atleast 2 columns `updated_at` and `created_at` in every table to know when an operation is last performed on a row.
>   - Always give you own custom meaningful names to your constraints. It will be helpful for debugging.

```sql
CREATE TABLE employee (
    emp_id INT AUTO_INCREMENT,
    fname VARCHAR(40) NOT NULL,
    lname VARCHAR(20) NOT NULL,
    email VARCHAR(50) UNIQUE,
    salary DECIMAL(10,2) CHECK (salary > 0.0),
    emp_status ENUM('Active', 'Inactive') DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP -- tell sql to put current timestamp on updation of row
    CONSTRAINT emp_id_pk PRIMARY KEY (emp_id)
);
```

> **NOTE:** In case of `AUTO_INCREMENT` when multiple users tries to insert data then MySQL reserves some ids which gets auto incremented even if the `INSERT` query fails, that is why sometimes we see gap in ids.

2. **INSERT:**
    - used to insert data into table
    - reserved keyword
    ```sql
    -- Inserting Data for respective columns : SAFE
    INSERT INTO table_name (column1, column2, column3, ..., columnN)
    VALUES (value1, value2, value3, ..., valueN);

    -- Inserting Data for all columns : RISKY 
    INSERT INTO table_name
    VALUES (value1, value2, value3, ..., valueN);

    -- Inserting multiple rows for respective columns : SAFE
    INSERT INTO table_name (column1, column2, column3, ..., columnN)
    VALUES (row1_value1, row1_value2, row1_value3, ..., row1_valueN),
    (row2_value1, row2_value2, row2_value3, ..., row2_valueN),
    (row3_value1, row3_value2, row3_value3, ..., row3_valueN),
    ...
    (rowN_value1, rowN_value2, rowN_value3, ..., rowN_valueN);
    ```

3. **ALTER:**
    - used to modify table structure/schema
    - reserved keyword
    ```sql
    -- Format
    ALTER TABLE table_name some_action;

    -- Adding Column
    ALTER TABLE table_name ADD column_name column_type constraint;

    -- Modifying Column
    ALTER TABLE table_name MODIFY column_name column_type constraint;

    -- Dropping Column
    ALTER TABLE table_name DROP column_name;
    ```
    - In case of constraint column like `NOT NULL` we need to provide some default value for those columns else it will throw error and command will fail.
    - If we are not having nay `NOT NULL` constraint then MySQL will automatically add `NULL` values in that column for respective records.

4. **DROP:**
    - it is used to delete schema/structure as well as data associated with the table.
    - it has various usage like db drop, table drop, column drop, constraint drop, index drop, etc.
    - reserved keyword
    ```sql
    -- Format
    DROP TABLE IF EXISTS table_name;

    -- Drop Database
    DROP DATABASE IF EXISTS db_name;

    -- Drop Column
    ALTER TABLE table_name DROP column_name;
    ```

> NOTE: All `CONSTRAINTS` have their unique name across a database and we should provide constraints name for each type of constraint as it makes debugging easier. If not provided then MySQl gives its own name for those constraints.

## Sample Code
```sql
-- Create table employee
CREATE TABLE employee (
    emp_id INT AUTO_INCREMENT,
    fname VARCHAR(40) NOT NULL,
    lname VARCHAR(20) NOT NULL,
    email VARCHAR(50) UNIQUE,
    salary DECIMAL(10,2) CHECK (salary > 0.0),
    emp_status ENUM('Active', 'Inactive') DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- tell sql to put current timestamp on updation of row
    CONSTRAINT emp_id_pk PRIMARY KEY (emp_id)
);

-- Inserting Data in Employee
INSERT INTO employee (fname, lname, email, salary, emp_status)
VALUES ('John', 'Doe', 'john.doe@example.com', 50000.00, 'Active'),
('Jane', 'Doe', 'jane.doe@example.com', 60000.00, 'Active');

-- Alter Table and add column updated_by
ALTER TABLE employee ADD updated_by VARCHAR(40);

-- Modifying Column updated_by to NOT NULL with default value 'NONE'
-- But if table have data with update_by as NULL then it will throw error as 'SQL Error [1138] [22001]: Data truncation: Invalid use of NULL value'
-- so we need to update it first
UPDATE employee SET updated_by = 'NONE' WHERE updated_by IS NULL;
ALTER TABLE employee MODIFY updated_by VARCHAR(40) NOT NULL DEFAULT 'NONE';

-- Drop Column updated_by
ALTER TABLE employee DROP updated_by;

-- Drop Table employee
DROP TABLE IF EXISTS employee;
```