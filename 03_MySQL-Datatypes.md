# MySQL Datatypes : NUMERIC, STRING, DATETIME and BINARY


1. **Numeric Datatypes:**
    - **TINYINT:**
        - range(-128 to 127) or (0 to 255).
        - Categorized as *Integers*.
    - **SMALLINT:**
        - range(-32768 to 32767) or (0 to 65535).
        - Categorized as *Integers*.
    - **MEDIUMINT:**
        - range(-8388608 to 8388607) or (0 to 16777215).
        - Categorized as *Integers*.
    - **INT/INTEGER:**
        - range(-2147483648 to 2147483647) or (0 to 4294967295).
        - Categorized as *Integers*.
    - **BIGINT:**
        - range(-9223372036854775808 to 9223372036854775807) or (0 to 18446744073709551615).
        - Categorized as *Integers*.
    - **FLOAT:**
        - range(-3.402823466E+38 to -1.175494351E-38) or (1.175494351E-38 to 3.402823466E+38).
        - Categorized as *Floating Point*(gives approx values).
    - **DOUBLE:**
        - range(-1.7976931348623157E+308 to -2.2250738585072014E-308) or (2.2250738585072014E-308 to 1.7976931348623157E+308).
        - Categorized as *Floating Point*(gives approx values).
    - **DECIMAL(p,s):**
        - defined by precision(p) and scale(s) where p<=65 and s<=30. If p is not specified then by default it is 10 and if s is not specified then by default it is 0.
        - Categorized as *Fixed Point*(gives exact/precise values).

2. **String Datatypes:**
    - **CHAR(n):** 
        - max length is 255 characters
        - stores strings with a fixed length of n characters
        - right padded with spaces to fill n characters. Use sparingly, varchar is usually better.
        - if string is longer than n characters, it is truncated to n characters
        - storage is fixed n bytes
        best for fixed lengthed strings
        - Eg.: char(2) for US State code.
    - **VARCHAR(n):**
        - max length is 65535 bytes
        - storage is string_length + 1 or 2 bytes
        - best for variable lengthed strings,text fields.
        - most commonly used String type. Length is in bytes and not in characters.
        - Eg.: varchar(255) for full name.
    - **TINYTEXT:**
        - max length is 255 bytes
        - storage is string_length + 1 bytes
        - best for short text strings without need for index.
        - cannot have default value, rarely used, varchar is preferred.
        - Eg.: short comments or notes.
    - **TEXT:**
        - max length is 65535 bytes
        - storage is string_length + 2 bytes
        - best for long text articles or descriptions.
        - cannot be a part of index without length specification.
        - Eg.: product description.
    - **MEDIUMTEXT:**
        - max length is 16 MB
        - storage is string_length + 3 bytes
        - best for large text documents.
        - consider performance impact on queries.
        - Eg.: blog posts, long articles, etc.
    - **LONGTEXT:**
        - max length is 4 GB
        - storage is string_length + 4 bytes
        - best for very large text documents.
        - use with caution as it impacts performance significantly.
        - Eg.: document storage, large text dumps, etc.
    - **ENUM('val1','val2',...,'valN'):**
        - max length is 65535 members
        - storage is 1 or 2 bytes
        - best for single selection from fixed list of values.
        - consider varchar if list might change dynamically.
        - Eg.: enum('M','F') for gender.
    - **SET('val1','val2',...,'valN'):**
        - max length is 64 members
        - storage is 1, 2, 3, 4, or 8 bytes
        - best for multiple selection from fixed list of values.
        - can store multiple values so use with caution.
        - Eg.: set('Black', 'Blue', 'Red', 'Green', 'Yellow', 'White') for available colors of a product.

> **NOTE:** *VARCHAR* is stored inside table while *TEXT* is stored outside table and a pointer to that gtext is stored in table but when retrieved the output will be text and not pointer.

> **NOTE:** *ENUM* stores *enum* data in table metadata and respected integer corresponding to the *enum* value will be stored as row inside table i.e., no text is stored.

```sql
CREATE TABLE product_details (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    product_code CHAR(10),
    product_name VARCHAR(100),
    short_descr TINYTEXT,
    detailed_descr TEXT,
    add_info MEDIUMTEXT,
    full_manual LONGTEXT,
    price DECIMAL(10,2),
    prd_size ENUM('S', 'M', 'L', 'XL', 'XXL'),
    avl_color SET('Black', 'Blue', 'Red', 'Green', 'Yellow', 'White')
);
```

3. **Datetime Datatypes:**
    - **DATE:**
        - storage is 3 bytes
        - stores date in format YYYY-MM-DD
        - range is from '1000-01-01' to '9999-12-31'
        - Eg.: '2022-12-03'
    - **TIME:**
        - storage is 3 bytes
        - stores time in format HH:MM:SS
        - range is from '-838:59:59' to '838:59:59'
        - Eg.: '12:34:56'
    - **YEAR:**
        - storage is 1 byte
        - stores year in format YYYY
        - range is from 1901 to 2155
        - Eg.: '2022'
    - **DATETIME:**
        - storage is 8 bytes
        - stores date and time in format YYYY-MM-DD HH:MM:SS
        - range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'
        - Eg.: '2022-12-03 12:34:56'
    - **TIMESTAMP:**
        - storage is 4 bytes
        - it stores UTC based date time.
        - stores date and time in format YYYY-MM-DD HH:MM:SS
        - Eg.: '2022-12-03 12:34:56'
        
> **NOTE:** When a user fetches the data which is stored as *timestamp* then they will see *timestamp* of their timezone as it gets converted automatically.

```sql
CREATE TABLE event_schedule (
    evt_id INT AUTO_INCREMENT PRIMARY KEY,
    evt_name VARCHAR(100),
    evt_date DATE,
    evt_time TIME,
    evt_year YEAR,
    evt_datetime DATETIME,
    evt_timestamp TIMESTAMP
);
```

4. **Binary Datatypes:**
    - **BINARY(n):**
        - storage is 255 bytes
        - stores small binary data like key encryption or hashes.
        - right padded with null.
    - **VARBINARY(n):**
        - storage is 64 KB
        - stores small binary data like key encryption or hashes.
        - no padding
    - **TINYBLOB:**
        - storage is 255 bytes.
        - used to store files.
    - **BLOB:**
        - storage is 64 KB.
        - used to store files.
    - **MEDIUMBLOB:**
        - storage is 16 MB.
        - used to store files.
    - **LONGBLOB:**
        - storage is 4 GB.
        - used to store files.
> **NOTE:** *LOAD_FILE(path)* is used to insert file from filesystem in table as *blob*. *BLOB* is acronym for *Binary Large Object*.

```sql
CREATE TABLE file_storage (
    file_id INT AUTO_INCREMENT PRIMARY KEY,
    file_name VARCHAR(100),
    security_key BINARY(32),
    session_token VARBINARY(64),
    profile_picture TINYBLOB,
    cover_photo BLOB,
    document MEDIUMBLOB,
    file_data LONGBLOB
);
```