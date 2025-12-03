# SQL Basics: DDL, DML, DCL, TCL

### SQL:
- Structured Query Language
- Programming language that is used to manage and manipulate relational databases.

#### Main Characteristics of SQL Commands:
1. **Data Definition Language (DDL):** It provides commands to define the structure of a data base. [Table Level/Schema Level/Database Level]
    - **CREATE:** It creates new database, table, index, view, etc.
    - **ALTER:** It modifies existing database, table, index, view, etc.
    - **DROP:** It deletes existing database, table, index, view, etc.
    - **TRUNCATE:** It removes all data from a table.
    - **RENAME:** It renames a table.
    - **SHOW:** It displays information about database, table, index, view, etc.
    - **DESCRIBE:** It displays information about table.
2. **Data Manipulation Language (DML):** It provides commands to let you insert, update, delete and retrieve data within the database. [Data]
    - **SELECT:** It retrieves data from a database.
    - **INSERT:** It inserts new data into a table.
    - **UPDATE:** It updates existing data in a table.
    - **DELETE:** It deletes existing data from a table.
3. **Data Control Language (DCL):** It provides commands to control access to data in a database.[Rights/Permissions]
    - **GRANT:** It grants permissions to a user or role.
    - **REVOKE:** It revokes permissions from a user or role.
4. **Transaction Control Language (TCL):** It provides commands to manage transactions within a database, ensuring data integrity and consistency.
    - **COMMIT:** save all changes made during current transaction.
    - **ROLLBACK:** undo all changes made during current transaction.
    - **SAVEPOINT:** sets a point within a transaction where you can roll back to.
    - **SET TRANSACTION:** configure transaction properties.

#### Key Features of SQL:
1. **Simplicity:** It provides English like syntax which is easy to learn.
2. **Ubiquity:** It is supported by most of the RDBMS i.e., MySQL, PostgreSQL, Oracle, SQL Server, etc.
3. **Flexibility:** It can handle complex queries involving multiple joined tables, subqueries, window functions, etc.
4. **Performance:** When used properly with Indexes and Query Optimization, it can query large volumes of data very efficiently.
5. **Data Integrity:** Features like Transaction, Constraints(PK, FK, Unique, etc.) and ACID(Atomicity, Consistency, Isolation, Durability) compliance ensures that data remains accurate and consistent.

#### Basic Database Terminologies:
- **Schema:**
    - Logical blueprint or structure that defines how data is organized within a database.
    - Tables in a database
    - relationship between those tables
    - attributes within each table
    - constraints and rules that govern table
    - data type of each field

- **Types of Schema:**
    1. **Database Schema:** complete structure of entire DB
    2. **Table Schema:** structure of a specific table including column names, data types, and constraints.
    3. **Sub Schema:** a portion of database visible to specific user/application.

- **Key Points:**
    1. **Index:** like creating a dictionary from records make lookup of data faster.
    2. **Query:** request to retrieve or manipulate data in DB.
    3. **Result Set:** outcome received in tabular format for a query.