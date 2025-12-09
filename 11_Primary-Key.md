# MySQl: PRIMARY KEY

- it uniquely identifies each record in table.
- it ensures no duplicate records exist.
- it provides a reference point for relationships between tables.
- it is `UNIQUE` and `NOT NULL` by default.
- they optimize database performance for records retrieval.
- if we can't define a single column as primary key, we can define a composite primary key i.e., a primary key made up of multiple columns, and it is defined in table `CONSTRAINT` only.
- Best practices:
    - always include a primary key in every table.
    - use `AUTO_INCREMENT` unless you have specific reason not to.
    - keep `PRIMARY KEY` simple i.e., use `INT` or `BIGINT` data type for numerical ids.
- when we create a `PRIMARY KEY` in MySQL, the database engine implements it as a special type of index called *CLUSTERED INDEX*.
- *CLUSTERED INDEX* does storing data like dictionary i.e., data is stored on the basis of `PRIMARY KEY`.
    - only one clustered index per table.
    - it uses B/B+ Tree to store data.
    - it stores frequently accessed things in buffer pool such as PAGE.

## Sample Code
```sql
-- directly define primary key
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);

-- define primary key in table constraint
CREATE TABLE order (
    id INT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    CONSTRAINT order_pk PRIMARY KEY (id)
);

-- composite key
CREATE TABLE student (
    std_id INT,
    course_id INT,
    CONSTRAINT std_course_pk PRIMARY KEY (std_id, course_id)
);

-- alter table to add primary key
ALTER TABLE table_name  
ADD CONSTRAINT constraint_name PRIMARY KEY (column_name);
```