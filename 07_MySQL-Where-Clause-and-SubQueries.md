# MYSQL WHERE clause: NULL, LIKE, IN, BETWEEN, Sub Queries, Logical Operators and Comparison Operators

### LOGICAL OPERATORS
- **`AND`**: returns true if all conditions are true
- **`OR`**: returns true if any one condition is true
- **`NOT`**: returns true if condition is false i.e., negate condition

### COMPARISON OPERATORS
- Six types of comparison operators are available
    - `=`: equal to
    - `!=` or `<>`: not equal to
    - `>`: greater than
    - `<`: less than
    - `>=`: greater than or equal to
    - `<=`: less than or equal to
- These works with `Integer`, `String`, `Decimal` and `Date` types but in case of `String`, comparison will occur by lexographically i.e., dictionary order.
- By default it is case insensitive but for case sensitive comparison we can use `BINARY` keyword.
- When comparing `String` with `Number` it will convert `String` to `Number` i.e, implicit type conversion is done and then comparison is done.
- When there is combination of `String` and `Number` like *`21fcf`* then MySQl compares by reading till number part i.e., `21` and rest part if left out and comparison is done.

## WHERE CLAUSE

- *WHERE* clause is used to filter data
- it does comparison operations such as NULL checks, equality checks, multiple filters using Logical Operators
- we can do multiple filters by separating conditions using `()`.

```sql
SELECT * FROM table_name WHERE condition1;
SELECT * FROM table_name WHERE condition1 AND condition2 OR condition3;
SELECT * FROM table_name WHERE (condition1 AND condition2) OR condition3;
```

## NULL CHECKS
- `IS NULL`: checks if value is null
- `IS NOT NULL`: checks if value is not null

> **NOTE:** While doing `null` checks we have to use `IS NULL` or `IS NOT NULL` instead of `= NULL` or `!= NULL` because any logical operation on `null` gives `unknown or null` whereas `WHERE` clause works on `true(1)` or `false(0)`.

```sql
SELECT * FROM table_name WHERE column_name IS NULL;
SELECT * FROM table_name WHERE column_name IS NOT NULL;
```

## LIKE
- `LIKE` is used for pattern matching
- `%` is used to match any number of characters
- `_` is used to match single character only

```sql
SELECT * FROM table_name WHERE column_name LIKE 'pattern'; -- exact match
SELECT * FROM table_name WHERE column_name LIKE 'pattern%'; -- any number of characters after pattern
SELECT * FROM table_name WHERE column_name LIKE '%pattern'; -- any number of characters before pattern
SELECT * FROM table_name WHERE column_name LIKE '%pattern%'; -- any number of characters before and after pattern
SELECT * FROM table_name WHERE column_name LIKE 'pattern_'; -- exact 1 character after pattern
SELECT * FROM table_name WHERE column_name LIKE '_pattern'; -- exact 1 character before pattern
SELECT * FROM table_name WHERE column_name LIKE '_pattern_'; -- exact 1 character before and after pattern
```

### WILDCARDS
- `*` is used to match any number of characters, available in `WHERE` clause
- `%` is used to match any number of characters(0, 1, or more), available in `LIKE` clause
- `_` is used to match single character only(1), available in `LIKE` clause
- `[ ]` is used to match any character in the set, available in SQL Server.
- `[^ ]` or `[! ]` is used to match any character not in the set, available in `LIKE` clause
- `ESCAPE` or `\` is used to escape special characters (used when you want to match `%` or `_` literally), available in `LIKE` clause

- REGEXP Wildcards: `* + ? ^ $ []`

## BETWEEN
- filter data by including both bounds i.e., values lies within range

```sql
SELECT * FROM table_name WHERE column_name BETWEEN value1 AND value2;
```

## IN
 - filter data by matching any value in a given list

 ```sql
SELECT * FROM table_name WHERE column_name IN (value1, value2, ...);
```

## Sub Queries
- also known as nested query or inner query.
- it is query written inside another query.
- result of the inner query or subquery is used by the outer query.
- It is used when:
    1. you need result of 1 query inside another query.
    ```sql
    SELECT * FROM employee WHERE salary > (SELECT AVG(salary) FROM employee);
    ```
    2. filtering data based on another table result.
    ```sql
    SELECT * FROM employee WHERE department_id IN (SELECT department_id FROM department WHERE dept_location = 'New York');
    ```
    3. you need to perform operations that requires aggregation first.
    ```sql
    SELECT * FROM employee WHERE salary > (SELECT AVG(salary) FROM employee WHERE department = 'IT');
    ```
    4. when you can't easily use `JOIN` or query is becoming complex.
    ```sql
    SELECT * FROM employee WHERE department_id IN (SELECT department_id FROM department WHERE dept_location = 'New York');
    ```
    5. when updating or deleting rows conditionally.
    ```sql
    UPDATE employee SET salary = salary * 1.10 WHERE emp_id IN (SELECT emp_id FROM employee WHERE department = 'IT');

    DELETE FROM employee WHERE emp_id IN (SELECT emp_id FROM employee WHERE department = 'HR');
    ```
- It is NOT used when:
    1. `JOIN` can do the same thing more efficiently.
    ```sql
    SELECT * FROM employee JOIN department ON employee.department_id = department.department_id WHERE department.dept_location = 'New York';
    ```
    2. subqueries are nested deeply.
    ```sql
    SELECT * FROM employee WHERE emp_id IN (SELECT emp_id FROM employee WHERE department = 'IT' AND salary > (SELECT AVG(salary) FROM employee WHERE department = 'IT'));
    ```
    3. dealing with large datasets, in that case use *CTE(Common Table Expressions)* or `JOIN`.
    ```sql
    WITH cte_name AS (
        SELECT * FROM table_name WHERE some_condition
    )
    SELECT * FROM cte_name;
    ```

## Sample Code
```sql
CREATE TABLE author (
    author_id INT AUTO_INCREMENT.
    author_name VARCHAR(50) NOT NULL,
    author_email VARCHAR(50) UNIQUE,
    author_age INT CHECK (author_age > 0),
    author_gender ENUM('Male', 'Female', 'Other') DEFAULT 'Other',
    author_address VARCHAR(100),
    author_phone VARCHAR(15),
    author_status ENUM('Active', 'Inactive') DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CONSTRAINT author_id_pk PRIMARY KEY (author_id)
);

INSERT INTO author (author_name, author_email, author_age, author_gender, author_address, author_phone, author_status)
VALUES ('Paulo Coelho', 'paulo.coelho@example.com', 30, 'Male', '123 Main St', '123-456-7890', 'Active'),
('JK Rowling', 'jk.rowling@example.com', 25, 'Female', '456 Elm St', '987-654-3210', 'Active'),
('Stan Lee', 'stan.lee@example.com', 35, 'Male', '789 Oak St', '555-123-4567', 'Inactive'),('James Thurber', 'james.thurber@example.com', 40, 'Male', '123 Main St', '123-456-7890', 'Active');

CREATE TABLE book (
    book_id INT AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    author_id INT,
    publisher VARCHAR(50),
    published_date DATE,
    price DECIMAL(10, 2),
    category VARCHAR(50),
    stock INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CONSTRAINT book_author_fk FOREIGN KEY (author_id) REFERENCES author(author_id),
    CONSTRAINT book_id_pk PRIMARY KEY (book_id)
);

INSERT INTO book (title, author_id, publisher, published_date, price, category, stock)
VALUES
('Harry Potter', 1, 'Bloomsbury', '2022-01-01', 19.99, 'Fantasy', 10),
('The Alchemist', 2, 'HarperCollins', '2022-02-01', 29.99, 'Fantasy', 20),
('The Secret Life of Walter Mitty', 4, 'Penguin', '2022-03-01', 14.99, 'Humour | Short Story', 15),
('Iron Man', 3, 'Marvel Comics', '2022-04-01', 19.99, 'Science Fiction', 10)
;

SELECT * FROM book;
SELECT * FROM author;

-- Get book which is either fiction or price is less than 20
SELECT * FROM book WHERE category = 'Fantasy' OR price < 20;

-- Get book which is either fiction or price is less than 20 and author is Paulo Coelho
SELECT * FROM book WHERE category = 'Fantasy' OR price < 20 AND author_id = 1;

-- Get book which is either fiction or science fiction and price is less than 20
SELECT * FROM book WHERE (category = 'Fantasy' OR category = 'Science Fiction') AND price < 20;

-- Get book whose author is null
SELECT * FROM book WHERE author_id IS NULL;

-- Get books whose author is not null
SELECT * FROM book WHERE author_id IS NOT NULL;

-- Get book whose title has `The` in the beginning and case sensistive comparison
SELECT * FROM book WHERE title LIKE BINARY 'The%';

-- Get book whose title has `The` in the beginning and case insensistive comparison
SELECT * FROM book WHERE title LIKE 'The%';

-- Get book whose title has `The` in the middle and case insensistive comparison
SELECT * FROM book WHERE title LIKE '%The%';

-- Get book whose title has `The` in the end and case insensistive comparison
SELECT * FROM book WHERE title LIKE '%The';

-- Get book which has title only 5 characters long
SELECT * FROM book WHERE title LIKE '_____';

-- Get books whose price is betwwen 10 and 30
SELECT * FROM book WHERE price BETWEEN 10 AND 30;

-- Get books whose categories are in ('Fantasy', 'Science Fiction')
SELECT * FROM book WHERE category IN ('Fantasy', 'Science Fiction');

-- Get books whose price is more than average price
SELECT * FROM book WHERE price > (SELECT AVG(price) FROM book);

-- Get books whose authors are available in author table
SELECT * FROM book WHERE author_id IN (SELECT author_id FROM author);

-- Get books whose authors are not available in author table
SELECT * FROM book WHERE author_id NOT IN (SELECT author_id FROM author);

-- Get authors whose book price is more than average price
SELECT * FROM author WHERE author_id IN (SELECT author_id FROM book WHERE price > (SELECT AVG(price) FROM book));

-- Delete books whose author is not availbe in author table
DELETE FROM book WHERE author_id NOT IN (SELECT author_id FROM author);

```