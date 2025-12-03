# MySQL : Database Explained

### Database
- Organized collections of structured information or data, typocally stored electronically in a computer system.
- Allows for data to be easily managed, accessed, modified, updated and deleted.
- Backbone of various applications.

#### Key Characteristics of Databases:
1. **Persistent Storage:** data stored over long term by surviving system reboots and application restarts.
2. **Structured and Organized:**  data is systematically arranged to avoid duplication and inconsistency.
3. **Concurrent Access:** multiple users and application can use database simultaneously without corrupting the data.
4. **Easily Retrievable:** efficient methods exist for querying, filtering and retrieving stored data quickly.
5. **Security and Integrity:** access can be controlled and data can be protected against unauthorized use or corruption.

### Database Management System (DBMS)
- Software that manages databases, handling data storage, retrieval, updates and security.
- Acts as interface between databases and user/application.
- RDBMS: MySQL, PostgreSQL, Oracle, SQL Server.
- NoSQL: MongoDB, Cassandra, DynamoDB.
- In-Memory: Redis, Memcached.

#### Key Concepts in RDBMS:
- **Table:** It represents an entity or concept, also known as *Relation*. It consists of ***rows*** and ***columns***.
- **Row:** It represents a single record or instance of the entity or concept. It is also know as *Tuple* or *Record*.
- **Column:** It represents a single attribute or characteristic of the entity or concept. It is also know as *Field* or *Attribute*.
- **Primary Key:** It is a unique identifier for each row in the table. It is also know as *Key* or *Index*. It is used to ensure data integrity and to speed up data retrieval. It should be unique and not null.
- **Foreign Key:** It is a column or set of columns in one table that links to the primary key of another table. It is used to establish relationships between tables and enforce referential integrity.
- **Cardinality:** It represents the number of rows in a table. It is also know as *Cardinality* or *Multiplicity*. Also if relationship between table then it can be one-to-one, one-to-many, many-to-one or many-to-many.

### Relational Model
1. **Data Integrity:** It means no orphaned record exists and it can be achieved using primary key and foreign key.
2. **Reduced Redundancy:** It minimizes duplication. It ensures consistent data. It can be achieved using normalization.
3. **Flexibility in Query:** It provides declarative way to retrieve and manipulate data without changing design.