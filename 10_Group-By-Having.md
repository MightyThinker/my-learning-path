# MySQl: GROUP BY and HAVING

## GROUP BY
- it is used to group rows that have same values in one or more columns into set of summary rows.
- it is always used with aggregate functions to perform calculations within each group.
- any column in `SELECT` that is not part of aggregate fumction must be included in `GROUP BY` because `SELECt` returns one row per group and non aggregated columns must have single defined value for each group i.e., all non aggregate columns in `SELECT` must be part of `GROUP BY`.
- `WHERE` clause can not come after `GROUP BY`.

## HAVING
- it is used in conjunction with `GROUP BY` to filter groups based on aggregate values.
- if you want to filter before grouping then use `WHERE` clause but if you want to filter after grouping then use `HAVING`.

> **NOTE:** MySQL reads all records for which ypu are doing `GROUP BY` and makes a table in memory and if there are multiple rows then the KEY will be of both columns and VALUES stord are aggregated data from both columns.

## SAMPLE CODE
```sql
-- Format Example
SELECT column1, column2, aggregate_function(column3)
FROM table_name
WHERE condition
GROUP BY column1, column2
HAVING aggregate_function(column3) > some_value;

-- Dummy Table
CREATE TABLE sales (
    id INT PRIMARY KEY,
    product_name VARCHAR(50),
    quantity INT,
    price DECIMAL(10, 2)
);

-- Inserting Data
INSERT INTO sales (id, product_name, quantity, price) VALUES
(1, 'Product A', 10, 100.00),
(2, 'Product B', 5, 200.00),
(3, 'Product C', 20, 150.00),
(4, 'Product D', 15, 250.00),
(5, 'Product E', 25, 120.00);

-- Query
SELECT product_name, SUM(quantity) as total_quantity, AVG(price) as average_price
FROM sales
GROUP BY product_name
HAVING total_quantity > 10;

SELECT count(*),
    CASE
        WHEN price > 100 THEN 'Expensive'
        WHEN price > 50 AND price <= 100 THEN 'Moderate'
        ELSE 'Cheap'
    END as price_category
FROM sales
GROUP BY price_category
HAVING count(*) > 2;

-- Without aggregation we can have DISTINCT like result
SELECT product_name
FROM sales
GROUP BY product_name;
```