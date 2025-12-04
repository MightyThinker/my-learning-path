# MySQL: ORDER BY(Single Field, Multiple Fields, Function Field, Case, Null Handling), LIMIT and OFFSET

- Sample Dummy Data
```sql
CREATE TABLE book (
    book_id INT AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    publisher VARCHAR(50),
    published_date DATE,
    price DECIMAL(10, 2),
    category VARCHAR(50),
    stock INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CONSTRAINT book_id_pk PRIMARY KEY (book_id)
);

INSERT INTO book (title, publisher, published_date, price, category, stock)
VALUES
('Harry Potter', 'Bloomsbury', '2022-01-01', 19.99, 'Fantasy', 10),
('The Alchemist', 'HarperCollins', '2022-02-01', 29.99, 'Fantasy', 20),
('The Secret Life of Walter Mitty', 'Penguin', '2022-03-01', 14.99, 'Humour | Short Story', 15),
('Iron Man', 'Marvel Comics', '2022-04-01', 19.99, 'Science Fiction', 10)
;
```

## ORDER BY:
- It is used to sort query results.
- By default it is `ASC` i.e., ascending order.
- We can use `DESC` to sort in descending order.
- For single field sorting
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY column_name [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY price ASC;
    ```
- For multiple field sorting
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY column_name1 [ASC|DESC], column_name2 [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY price ASC, title DESC;
    ```
- sorting by column position : NOT RECOMMENDED
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY position [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY 2 ASC;
    ```
- sorting with `WHERE` clause
    ```sql
    -- Format
    SELECT * FROM table_name WHERE some_condition ORDER BY column_name [ASC|DESC];

    -- Example
    SELECT * FROM book WHERE price > 15 ORDER BY price ASC;
    ```
- sorting with function field
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY function(column_name) [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY LENGTH(title) ASC;
    ```
- custom sorting by field
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY FIELD(column_name, value1, value2, ...);

    -- Example
    SELECT * FROM book ORDER BY FIELD(category, 'Fantasy', 'Science Fiction', 'Humour | Short Story') ASC;
    ```
- custom sorting with condition
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY condition_expression [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY (price > 15 AND stock > 0) ASC;
    ```
- sorting with `CASE` expression
    - since the last example is not much readable so we can use `CASE` expression
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY 
        CASE 
            WHEN condition1 THEN sorting_priority1
            ...
            [WHEN conditionN THEN sorting_priorityN]
            ELSE default_sorting_priority
        END [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY
        CASE
            WHEN price > 15 AND stock > 0 THEN 1
            ELSE 2
        END ASC;
    ```
- sorting with null values
    - by default `NULL` values are sorted in `ASC` order i.e., they will come on top
    - we can use `NULLS FIRST` or `NULLS LAST` to control the position of `NULL` values
    - use `CASE` or condition expression using `IS` or `IS NOT` to control the position of `NULL` values
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY column_name [ASC|DESC] NULLS FIRST|LAST;
    SELECT * FROM table_name ORDER BY CASE WHEN column_name IS NULL THEN 1 ELSE 0 END [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY price ASC NULLS FIRST;
    SELECT * FROM book ORDER BY price ASC NULLS LAST;
    SELECT * FROM book ORDER BY CASE WHEN price IS NULL THEN 1 ELSE 0 END ASC;
    SELECT * FROM book ORDER BY CASE WHEN price IS NOT NULL THEN 1 ELSE 0 END ASC;
    ```
- sorting with calculated columns
    ```sql
    -- Format
    SELECT * FROM table_name ORDER BY (column_name + column_name) [ASC|DESC];

    -- Example
    SELECT * FROM book ORDER BY (price * stock) DESC;
    -- or we can use alias
    SELECT *, price*stock AS total FROM book ORDER BY total DESC;
    ```
> **NOTE:** To optimize a SQL Query we can do indexing.

> **NOTE:** If you want to know or debug a SQL working the we can use `EXPLAIN` keyword.
>   ```sql
>   EXPLAIN SELECT * FROM table_name ORDER BY column_name [ASC|DESC];
>   ```
> When we `ORDER BY` a column which is not index then it uses `FILE SORT ALGORITHM`.

## LIMIT
- used to control the number of records returned by a query
    ```sql
    -- Format
    SELECT * FROM table_name LIMIT number_of_record_desired;

    -- Example
    SELECT * FROM book LIMIT 5;
    SELECT * FROM book ORDER BY price DESC LIMIT 5;
    ```
## OFFSET
- used to skip a number of records from the start
- ideal for pagination and used with `LIMIT`
    ```sql
    -- Format
    SELECT * FROM table_name LIMIT number_of_record_desired OFFSET number_of_record_to_skip;

    -- Example
    SELECT * FROM book LIMIT 5 OFFSET 2;
    ```
- alternate simplified command
    ```sql
    -- Format
    SELECT * FROM table_name LIMIT [offset], [number_of_record_desired]; -- LIMIT [how much records to skip], [how much records to return]

    -- Example
    SELECT * FROM book LIMIT 2, 5;
    ```
- pagination formula
    ```sql
    -- Format
    SELECT * FROM table_name LIMIT [(page_number - 1) * items_per_page], [items_per_page];

    -- Example
    SELECT * FROM book ORDER BY price LIMIT 0,2; -- Page 1
    SELECT * FROM book ORDER BY price LIMIT 2,2; -- Page 2
    SELECT * FROM book ORDER BY price LIMIT 4,2; -- Page 3
    ```

> **NOTE:** When table has big value or large number of rows then using `OFFSET` will cause performance issue as it has to index the said offset rows.
> ```sql
> SELECT * FROM book ORDER BY book_id LIMIT 1000000, 10;
> ```
> In the above example it will index 1000000 rows and then return 10 rows which is not efficient. So we can use the last returned value to compare in `WHERE` clause to optimize it.
> ```sql
> -- Suppose if in earlier query if the last returned result set the last book_id is 1000000 then we can use it to compare in `WHERE` clause
> SELECT * FROM book WHERE book_id > 1000000 LIMIT 10;
> ```
> This way we can improve the performance of the query.

### Sample Code
```sql
-- Fetch book with highest price
SELECT * FROM book ORDER BY price DESC LIMIT 1;

-- Fetch book with lowest price
SELECT * FROM book ORDER BY price ASC LIMIT 1;

-- Top 3 most expensive book
SELECT * FROM book ORDER BY price DESC LIMIT 3;

-- Get 5 random books
SELECT * FROM book ORDER BY RAND() LIMIT 5;

-- Get 2nd highest price
SELECT DISTINCT price FROM book ORDER BY price DESC LIMIT 1 OFFSET 1;
```