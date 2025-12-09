# MySQL: Normalization

- Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity.

```sql
CREATE TABLE book_orders (
    order_id INT,
    customer_name VARCHAR(100),
    customer_address VARCHAR(100),
    customer_email VARCHAR(100),
    book_isbn VARCHAR(13),
    book_title VARCHAR(100),
    book_author VARCHAR(100),
    book_price DECIMAL(10,2),
    order_date DATE,
    quantity INT,
    total_price DECIMAL(10,2)
);

INSERT INTO book_orders VALUES
(1,'John Doe','123 Main St','john@example.com','978-3-16-148410-0','Book 1','Author 1',19.99,'2023-10-01',2,39.98),
(2,'Jane Smith','456 Elm St','jane@example.com','978-1-23-456789-0','Book 2','Author 2',29.99,'2023-10-02',1,29.99),
(3,'Bob Johnson','789 Oak St','bob@example.com','978-0-98-765432-1','Book 3','Author 3',14.99,'2023-10-03',3,44.97),
(4,'Alice Brown','321 Pine St','alice@example.com','978-2-34-567890-1','Book 1','Author 1',19.99,'2023-10-04',2,39.98);
```
- In above table we have everything in one table and suppose if we change the book price then we have to change it in every row, or if we change customer email then we have to change it in every row which is like redundant data and cost us redundant changes or operations so we can say that this is not normalized.

## 1st Normal Form (1NF)
- Each column contains atomic(indivisible) values i.e., values that cannot be further divided.
- Each column contains values of the same type.
- Each row is unique typically ensured by a `PRIMARY KEY`.
- no repeating groups of columns.

```sql
-- Let's convert above table to 1NF
CREATE TABLE book_orders_1NF (
    order_id INT,
    customer_name VARCHAR(100),
    customer_address VARCHAR(100),
    customer_email VARCHAR(100),
    book_isbn VARCHAR(13),
    book_title VARCHAR(100),
    book_author VARCHAR(100),
    book_price DECIMAL(10,2),
    order_date DATE,
    quantity INT,
    total_price DECIMAL(10,2)

    CONSTRAINT book_pk PRIMARY KEY (order_id, book_isbn)
);
```

## 2nd Normal Form (2NF)
- It should be in 1NF.
- All non-key attributes must be fully functionally dependent on the entire `PRIMARY KEY` i.e., partial dependencies are not allowed i.e., if we have a table with multiple columns and one of the columns is dependent on only a subset of the `PRIMARY KEY` then it is not in 2NF.
- In above table we have `order_date` and `quantity` are dependent on `order_id` and not on `book_isbn` so it is not in 2NF.

```sql
-- Let's convert it in 2NF
CREATE TABLE books_2NF(
    book_isbn VARCHAR(13),
    book_title VARCHAR(100),
    book_author VARCHAR(100),
    book_price DECIMAL(10,2),
    CONSTRAINT book_pk PRIMARY KEY (book_isbn)
);

CREATE TABLE orders_2NF(
    order_id INT,
    customer_name VARCHAR(100),
    customer_address VARCHAR(100),
    customer_email VARCHAR(100),
    order_date DATE,
    CONSTRAINT order_pk PRIMARY KEY (order_id)
);

CREATE TABLE order_items_2NF(
    order_id INT,
    book_isbn VARCHAR(13),
    quantity INT,
    total_price DECIMAL(10,2),
    CONSTRAINT order_items_pk PRIMARY KEY (order_id, book_isbn),
    CONSTRAINT order_items_fk FOREIGN KEY (order_id) REFERENCES orders_2NF(order_id),
    CONSTRAINT book_items_fk FOREIGN KEY (book_isbn) REFERENCES books_2NF(book_isbn)
);
```

## 3rd Normal Form (3NF)
- It should be in 2NF.
- It must not have transitive dependencies i.e., if we have a non-key attributes depends upon another non-key attribute rather than depending directly on the `PRIMARY KEY` then it is not in 3NF.
- In above table we have `customer_email` and `customer_address` depends upon `customer_name` and not on `order_id`, also `quantity` depends on `book_isbn` but `total_price` depends on `quntity` and `book_price` of another table so it is not in 3NF.

```sql
-- Let's convert it in 3NF
CREATE TABLE customer_3NF(
    customer_id INT,
    customer_name VARCHAR(100),
    customer_address VARCHAR(100),
    customer_email VARCHAR(100),
    CONSTRAINT customer_pk PRIMARY KEY (customer_id)
);

CREATE TABLE books_3NF(
    book_isbn VARCHAR(13),
    book_title VARCHAR(100),
    book_author VARCHAR(100),
    book_price DECIMAL(10,2),
    CONSTRAINT book_pk PRIMARY KEY (book_isbn)
);

CREATE TABLE orders_3NF(
    order_id INT,
    customer_id INT,
    order_date DATE,
    CONSTRAINT order_pk PRIMARY KEY (order_id),
    CONSTRAINT order_fk FOREIGN KEY (customer_id) REFERENCES customer_3NF(customer_id)
);

CREATE TABLE order_items_3NF(
    order_id INT,
    book_isbn VARCHAR(13),
    CONSTRAINT order_items_pk PRIMARY KEY (order_id, book_isbn),
    CONSTRAINT order_items_fk FOREIGN KEY (order_id) REFERENCES orders_3NF(order_id),
    CONSTRAINT book_items_fk FOREIGN KEY (book_isbn) REFERENCES books_3NF(book_isbn)
);
```

> **NOTE:**
>   - There are more further normal forms like `BCNF`, `4NF`, `5NF` but `3NF` is most used and recommended becuase till here it satisfies all the requirements of normalization and as more normal forms are introduced there will be increase in number of tables and requires more `JOIN` operations and it will be complex to maintain.
>   - `BCNF` alo known as `Boyce-Codd Normal Form` ensures every determinant(one column or set of columns) must be a `Candidate Key`.
>   - `4 NF` means no table contains two or more independent multi-valued dependencies.
>   - `Candidate Key` is a minimal set of columns that can uniquely identify each row in a table. Each candidate key can serve as a primary key, and no attribute in a candidate key can be removed without losing uniqueness.
