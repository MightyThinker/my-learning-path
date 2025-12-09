# MySQL: FOREIGN KEY

- A `FOREIGN KEY` is column or set of columns in one table that refers to the `PRIMARY KEY` column in another table.
- It creates a link between 2 tables, establishing a parent-child relationship.
    - Parent table: contains the primary key that is referenced.
    - Child table: contains the foreign key that references the primary key of Parent table.
- Purpose of Foreign Keys:
    - *Referential Integrity*: relationship between tables remains consistent.
    - *Structured Relationship*: clear and organized relationships in our database mirroring real world scenarios.
    - *Data Validation*: we achieve referential integrity like `INSERTION` and `DELETION` can't happen for data which is not there in parent table i.e., if data is referenced we can not delete it before removing the relationship and if data is not present in parent we can not reference it.
- Types of relationships:
    - *One-to-One(1:1)*: each record in table_1 relates to exactly one record in table_2.
    - *One-to-Many(1:M) or Many-to-One(M:1)*: each record in table_1 relates to multiple records in table_2.
    - *Many-to-Many(M:N)*: each record in table_1 relates to multiple records in table_2 and vice versa.

## Sample Code
```sql
-- One to One
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
CREATE TABLE student_details (
    id INT PRIMARY KEY,
    student_id INT,
    address VARCHAR(100),
    CONSTRAINT st_dt_fk FOREIGN KEY (student_id) REFERENCES student(id)
);


-- One-to-Many or Many-to-One
CREATE TABLE class (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    class_id INT,
    CONSTRAINT st_cl_fk FOREIGN KEY (class_id) REFERENCES class(id)
);

-- Many-to-Many
CREATE TABLE student (
    std_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
CREATE TABLE course (
    course_id INT PRIMARY KEY,
    name VARCHAR(100)
);
-- Junction Table which stores relationships between student and course
CREATE TABLE enrollments (
    std_id INT,
    course_id INT,
    CONSTRAINT std_course_pk PRIMARY KEY (std_id, course_id),
    CONSTRAINT std_course_fk FOREIGN KEY (std_id) REFERENCES student(std_id),
    CONSTRAINT course_course_fk FOREIGN KEY (course_id) REFERENCES course(course_id)
);

-- Add Contsraint
ALTER TABLE table_name
ADD CONSTRAINT constraint_name FOREIGN KEY (column_name) REFERENCES table_name(column_name);

-- Remove Contsraint
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;

-- If you don't know constraint then we can use SHOW CREATE TABLE to get constraint name
SHOW CREATE TABLE table_name;
```