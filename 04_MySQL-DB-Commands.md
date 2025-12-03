# MySQL DB Commands: CREATE, USE, DROP, SHOW

1. **CREATE:**
    - used to create database
    - case insensistive (the name of database) in Windows and case sensitive in Linux
    - reserved keyword
    ```sql
    CREATE DATABASE db_name; -- if database already exists then it will throw error
    CREATE DATABASE IF NOT EXISTS db_name; -- if database already exists then it will not throw error
    ```
    - it can be number, letters or underscore

2. **SHOW:**
    - used to list all databases
    - reserved keyword
    ```sql
    SHOW DATABASES;
    -- To show which database is selected we use below command:
    SELECT DATABASE();
    ```

3. **USE:**
    - used to select a database to work with.
    - reserved keyword
    ```sql
    USE db_name;
    ```

4. **DROP:**
    - used to delete database along with all its tables and data i.e., completely remove
    - reserved keyword
    ```sql
    DROP DATABASE db_name;
    -- To drop a database if it exists we use below command:
    DROP DATABASE IF EXISTS db_name;
    ```
## Sample Code:

```sql
-- Create a database named 'learning'
CREATE DATABASE IF NOT EXISTS learning;

-- Get list of databases
SHOW DATABASES;

-- select database to work with
USE learning;

-- get current working database
SELECT DATABASE();

-- drop database
DROP DATABASE IF EXISTS learning;

```