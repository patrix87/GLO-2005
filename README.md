# SQL ORDER OF EXECUTION
|Clause|Function|
|-|-|
|FROM, including JOINs|Choose and join tables|
|WHERE|Filter the base data|
|GROUP BY|Aggregates the base data|
|HAVING|Filters the aggregated data|
|SELECT|Returns the final data|
|DISTINCT|Remove duplicated rows|
|UNION|Combines multiple result set|
|ORDER BY|Sort the final data|
|LIMIT|Offset and limit the final data|

# SQL ORDER OF CLAUSES
|Clause|Function|
|-|-|
|SELECT|Select columns|
|DISTINCT|Remove duplicated rows|
|FROM|Select table|
|JOIN|Select another table|
|WHERE|Filter the data|
|GROUP BY|Aggregates the data|
|HAVING|Filters the aggregated data|
|UNION|Combines multiple result set|
|ORDER BY|Sort the final data|
|LIMIT|Offset and limit the final data|

# SQL CLAUSES IN WRITING ORDER

## USE
```sql
USE Database
```

Tell SQL to use a specific database.

# SELECT 
Returned columns

```sql
SELECT *
``` 

```sql
SELECT column + 10 * 2 as 'calculated column'
```

```sql
SELECT table.column, other_table.column AS 'column_name', 'string' AS "other column name"
```

```sql
SELECT DISTINCT *
```

### SUBQUERY IN SELECT

```sql
SELECT column, other_column, (
    SELECT SUM(column_x)
    FROM table_x
    WHERE column_y = 2
)
FROM table y;
```


## FROM
Target table

```sql
FROM table;
```

```sql
FROM database.table;
```

```sql
FROM table alias;
```

```sql
FROM (query);
```
### SUBQUERY IN FROM

```sql
SELECT *
FROM (
    SELECT SUM(column_x)
    FROM table_x x
    WHERE x.column_y = 2
) AS alias;
```
## JOIN
Join only rows that match conditions in both table.

## INNER JOINS

```sql
JOIN table ON table.column = other_table.column;
```

```sql
JOIN table alias ON alias.column = table.column;
```

### SELF JOIN

```sql
FROM same_table a JOIN same_table b ON a.column = b.column;
```

### MULTIPLE JOINS

```sql
JOIN table a ON a.column = other_table.column;
```

```sql
JOIN table b ON b.column = a.column;
```

```sql
JOIN table c ON c.column = other_table.other_column;
```

### COMPOUND JOIN CONDITIONS

```sql
JOIN table a ON a.column = b.column AND a.other_column = b.other_column;
```

### USING KEYWORD

```sql
JOIN table a USING (column);
```

```sql
JOIN table a USING (column, other_column);
```

### NATURAL JOIN
Will try to join on matching columns *discouraged*

```sql
NATURAL JOIN table a;
```

### IMPLICIT SYNTAX

```sql
FROM table a, table b WHERE a.column = b.column;
```

## OUTER JOINS

### LEFT JOIN
return all the results from the table on the left *(above)* of the JOIN and only the matching columns from the table on the right.

```sql
LEFT JOIN table a ON a.column = b.column;
```

### RIGHT JOIN

return all the results from the table on the right *(above)* of the JOIN and only the matching columns from the table on the left.

```sql
RIGHT JOIN table a ON a.column = b.column;
```

### SELF OUTER JOINS

```sql
FROM same_table a LEFT JOIN same_table b ON a.column = b.column;
```

## CROSS JOINS
return all combinations of both table. EG colors and size combo.

```sql
FROM table a CROSS JOIN table b;
```

### IMPLICIT SYNTAX

```sql
FROM table a, table b;
```

## WHERE
Conditions

```sql
WHERE column = 1;
```

Contains string
```sql
WHERE column like "%string%";
```

Compound condition
```sql
WHERE column = 3 AND other_column = 2;
```

Value is in range *(inclusive)
```sql
WHERE column BETWEEN 1 AND 10
```

Regular Expression
```sql
WHERE column REGEXP '([A-Z])\w+';
```
### WITH KEYWORD

```sql
WITH
    cte1 AS (
        SELECT a, b 
        FROM table1
    ),
    cte2 AS (
        SELECT c, d 
        FROM table2
    )
SELECT b, d 
FROM cte1 
JOIN cte2
WHERE cte1.a = cte2.c;
```


### ALL KEYWORD

```sql
WHERE column > ALL(1,2,3,4,5)
```

return all item that are greater than all the items in the subquery
```sql
WHERE column > ALL(
    SELECT values
    FROM table
    WHERE value > 10
)
```

### ANY KEYWORD

return all item that match the subquery
```sql
WHERE id = ANY(
    SELECT id
    FROM table
    WHERE value > 10
)
```

### IN KEYWORD

Value is in list
```sql
WHERE column IN ('value','other_value');
```

### EXISTS KEYWORD

Return all record that match the subquery
```sql
WHERE EXISTS(
    SELECT id
    FROM table
    WHERE value > 10
)
```

## GROUP BY
Group by column

```sql
GROUP BY column;
```

## HAVING KEYWORD

filters the aggregated data
```sql
GROUP BY column
HAVING column_a > 3;
```

## ORDER BY
Order values based on a column

```sql
ORDER BY column;
```

```sql
ORDER BY column DESC;
```

```sql
ORDER BY column DESC, other_column;
```

## LIMIT
Limit the number of returns

```sql
LIMIT 1000;
```

Skip 500, return 100

```sql
LIMIT 500,100;
```
# INSERT

### Based on column name
```sql
INSERT INTO table (
    firstname,
    lastname
)
VALUES (
    'value',
    'other value'
);
```

### Based on column order
```sql
INSERT INTO table
VALUES (
    DEFAULT,
    'value',
    'other value',
    NULL
);
```

### Multiple values
```sql
INSERT INTO table (
    name,
    password
)
VALUES  (bob, qwerty123),
        (alfred, chatton), 
        (john,xXRocker!Xx)
```

### Multiple tables
```sql
INSERT INTO table (firstname, lastname)
VALUES (bob, tremblay)

INSERT INTO order_id (client_id)
VALUES (LAST_INSERT_ID());
```
*both table have auto-increment id*

### Copy a table

```sql
CREATE TABLE new_table AS
SELECT * FROM table;
```

### Partial copy
```sql
CREATE TABLE new_table AS
SELECT * FROM table
TRUNCATE TABLE new_table;

INSERT INTO new_table
SELECT *
FROM table
WHERE column < 1;
```

### Join copy
```sql
CREATE TABLE new_table AS
SELECT * FROM table
TRUNCATE TABLE new_table;

INSERT INTO new_table
SELECT *
FROM table
JOIN other_table o ON o.id = new_table.id
WHERE column < 1;
```

# UPDATE

### Update single row
```sql
UPDATE table
SET column = 1, other_column = NULL
WHERE id = 0;
```

### Update multiple row
```sql
UPDATE table
SET column = 1, other_column = NULL
WHERE column = 0;
```

### Update with subqueries
```sql
UPDATE table
SET column = 1, other_column = NULL
WHERE column = (
    SELECT column_a
    FROM table
    WHERE column_b > 2000;
)
```

# DELETE

### Delete one or more rows

```sql
DELETE FROM table
WHERE column = 1;
```


# VIEWS

Create a view
```sql
CREATE OR REPLACE VIEW view_name AS
    SELECT * 
    FROM table 
    WHERE column;
```

Delete a view
```sql
DROP VIEW view_name;
```

# STORED PROCEDURE

Create a stored procedure
```sql
DELIMITER $$
CREATE PROCEDURE procedure_name(
    variable_name CHAR(10)
)
BEGIN
    SELECT *
    FROM table c
    WHERE c.column = variable_name;
END$$
DELIMITER ;
```

Validate variables
```sql
DELIMITER $$
CREATE PROCEDURE procedure_name(
    variable_name INT(10)
)
BEGIN
    IF variable_name > 9000 THEN
        SIGNAL SQLSTATE '22003'
            SET MESSAGE_TEXT = "IT'S OVER 9000";
    END IF;
    SELECT *
    FROM table c
    WHERE c.column = variable_name;
END$$
DELIMITER ;
```


Output values
```sql
DELIMITER $$
CREATE PROCEDURE procedure_name(
    variable_name INT(10)
    OUT return_value CHAR(24)
)
BEGIN
    SELECT name
    INTO return_value
    FROM table c
    WHERE c.column = variable_name;
END$$
DELIMITER ;
```


Delete a stored procedure
```sql
DROP PROCEDURE procedure_name;
```

Use a stored procedure
```sql
CALL procedure_name(variable);
```

Use a stored procedure with output parameters
```sql
SET @return_value = "";
CALL procedure_name(variable_name, @return_value);
SELECT @return_value;
```

# TRIGGERS

```sql
DELIMITER $$
CREATE TRIGGER table_after_insert
    AFTER INSERT ON table
    FOR EACH ROW
BEGIN
    UPDATE table_b
    SET table_b.value = NEW.table.value
    WHERE table_b.id = table.id
END$$
DELIMITER ;
```

Delete a stored procedure
```sql
DROP TRIGGER IF EXISTS table_after_insert;
```



# COMPARAISON OPERATORS

|Comparison Operator|Description|
|-|-|
|`=`|Equal|
|`<=>`|Equal (Safe to compare NULL values)|
|`<>`|Not Equal|
|`!=`|Not Equal|
|`>`|Greater Than|
|`>=`|Greater Than or Equal|
|`<`|Less Than|
|`<=`|Less Than or Equal|
|`NOT`|Negates a condition|
|`AND`|2 or more conditions to be met|
|`OR`|Any one of the conditions are met|
|`BETWEEN`|Within a range (inclusive)|
|`IS NULL`|NULL value|
|`IS NOT NULL`|Non-NULL value|
|`LIKE`|Pattern matching with % and _|
|`REGEXP`|Regular Expression matching|
|`IN ()`|Matches a value in a list|
|`ANY()`|Any of the values are matching|
|`ALL()`|The condition is applied against ALL the values|
|`EXISTS()`|The condition exists in the return value.|

# WILDCARDS

|Wildcard|Explanation|
|-|-|
|`%`|Allows you to match any string of any length (including zero length)|
|`_`|Allows you to match on a single character|

# AGGREGATE FUNCTIONS

|Function|Use|
|-|-|
|`COUNT()`|Count the number of rows that are not null unless used with COUNT(*). To exclude duplicate value count use COUNT(DISTINCT column)|
|`SUM()`|Calucalte the sum of all none null values|
|`AVG()`|Calucalte the average of all none null values|
|`MIN()`|Find the minimum value|
|`MAX()`|Find the maximum value|
|`IFNULL(column, 'value')`|Replace a null value with something else|
|`COALESCE(column, column, column)`|Return the first not null value|
|`IF(condition, return if true, return if false)`|Conditional return|

# CASE

Return a value based on one of the conditions
```sql
CASE 
    WHEN VALUE = 1 THEN 'ONE'
    WHEN VALUE = 2 THEN 'TWO'
    ELSE 'NUMBER'
END AS digit
```
# DATA TYPES

## String Data Types
|Data type|Description|
|-|-|
|`CHAR(size)`|A FIXED length string (can contain letters, numbers, and special characters). The size parameter specifies the column length in characters - can be from 0 to 255. Default is 1|
|`VARCHAR(size)`|A VARIABLE length string (can contain letters, numbers, and special characters). The size parameter specifies the maximum column length in characters - can be from 0 to 65535|
|`BINARY(size)`|Equal to CHAR(), but stores binary byte strings. The size parameter specifies the column length in bytes. Default is 1|
|`VARBINARY(size)`|Equal to VARCHAR(), but stores binary byte strings. The size parameter specifies the maximum column length in bytes.|
|`TINYBLOB`|For BLOBs (Binary Large OBjects). Max length: 255 bytes|
|`TINYTEXT`|Holds a string with a maximum length of 255 characters|
|`TEXT(size)`|Holds a string with a maximum length of 65,535 bytes|
|`BLOB(size)`|For BLOBs (Binary Large OBjects). Holds up to 65,535 bytes of data|
|`MEDIUMTEXT`|Holds a string with a maximum length of 16,777,215 characters|
|`MEDIUMBLOB`|For BLOBs (Binary Large OBjects). Holds up to 16,777,215 bytes of data|
|`LONGTEXT`|Holds a string with a maximum length of 4,294,967,295 characters|
|`LONGBLOB`|For BLOBs (Binary Large OBjects). Holds up to 4,294,967,295 bytes of data|
|`ENUM(val1, val2, val3, ...)`|A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them|
|`SET(val1, val2, val3, ...)`|A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list|

## Numeric Data Types
|Data type|Description|
|-|-|
|`BIT(size)`|A bit-value type. The number of bits per value is specified in size. The size parameter can hold a value from 1 to 64. The default value for size is 1.|
|`TINYINT(size)`|A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. The size parameter specifies the maximum display width (which is 255)|
|`BOOL`|Zero is considered as false, nonzero values are considered as true.|
|`BOOLEAN`|Equal to BOOL|
|`SMALLINT(size)`|A small integer. Signed range is from -32768 to 32767. Unsigned range is from 0 to 65535. The size parameter specifies the maximum display width (which is 255)|
|`MEDIUMINT(size)`|A medium integer. Signed range is from -8388608 to 8388607. Unsigned range is from 0 to 16777215. The size parameter specifies the maximum display width (which is 255)|
|`INT(size)`|A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. The size parameter specifies the maximum display width (which is 255)|
|`INTEGER(size)`|Equal to INT(size)|
|`BIGINT(size)`|A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 18446744073709551615. The size parameter specifies the maximum display width (which is 255)|
|`FLOAT(size, d)`|A floating point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter. This syntax is deprecated in MySQL 8.0.17, and it will be removed in future MySQL versions|
|`FLOAT(p)`|A floating point number. MySQL uses the p value to determine whether to use FLOAT or DOUBLE for the resulting data type. If p is from 0 to 24, the data type becomes FLOAT(). If p is from 25 to 53, the data type becomes DOUBLE()|
|`DOUBLE(size, d)`|A normal-size floating point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter|
|`DOUBLE PRECISION(size, d)`| |
|`DECIMAL(size, d)`|An exact fixed-point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter. The maximum number for size is 65. The maximum number for d is 30. The default value for size is 10. The default value for d is 0.|
|`DEC(size, d)`|Equal to DECIMAL(size,d)|

Note: All the numeric data types may have an extra option: UNSIGNED or ZEROFILL. If you add the UNSIGNED option, MySQL disallows negative values for the column. If you add the ZEROFILL option, MySQL automatically also adds the UNSIGNED attribute to the column.

## Date and Time Data Types
|Data type|Description|
|-|-|
|`DATE`|A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'|
|`DATETIME(fsp)`|A date and time combination. Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'. Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time|
|`TIMESTAMP(fsp)`|A timestamp. TIMESTAMP values are stored as the number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC). Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC. Automatic initialization and updating to the current date and time can be specified using DEFAULT CURRENT_TIMESTAMP and ON UPDATE CURRENT_TIMESTAMP in the column definition|
|`TIME(fsp)`|A time. Format: hh:mm:ss. The supported range is from '-838:59:59' to '838:59:59'|
|`YEAR`|A year in four-digit format. Values allowed in four-digit format: 1901 to 2155, and 0000. MySQL 8.0 does not support year in two-digit format.|

# TRANSACTION

Properties Of MySQL TRANSACTION
MySQL supports the ACID properties for a transaction-safe Relational Database Management System. Let’s see each of these properties in brief.
||||
|-|-|-|
|A|Atomicity|Transactions support atomicity by running ALL or NONE – i.e. either all the statements of a transaction would be executed or NONE of them.|
|C|Consistency|The Consistency property ensures that the database should be in a consistent state before and after the transaction is complete. This is supported by the atomic nature of the transaction.|
|I|Isolation|MySQL provides the concept of locks along with the transaction. This ensures that during transaction execution, no other operation can happen on that row of data.|
|D|Durability|Durability refers to the ability of the database to recover from failures. i.e. even if there are system failures, any transaction once successful should be able to apply the changes.|

Isolation level vs Concurency problems that they fix
|ISOLATION LEVEL|Lost Updates|Dirty Reads|Non-Repeatable Reads|Phantom Reads|
|-|-|-|-|-|
|READ UNCOMMITED||||
|READ COMMITED||X||
|REPEATABLE READ (Default MySQL level)|X|X|X||
|SERIALIZABLE|X|X|X|X|

### Set transaction level

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
SELECT * FROM table WHERE id = 1;
COMMIT;
```

### Rollback transaction

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
UPDATE table SET value = 1 WHERE column = 2;
ROLLBACK;
```

# NORMALISATION

## 1NF : First Normal Form

- Each cell should have a single value. *(Array of item in a column)
- Columns should never repeat. *(item_1, item_2, item_3).

## 2NF : Second Normal Form

- Must be in the 1NF
- Every table should describe only one entity.
- Every column in that table should describe that entity only.

## 3NF : Third Normal Form

- Must be in the 2NF
- A column in a table should not be derived from other columns

# DATABASES

create database
```sql
CREATE DATABASE IF NOT EXISTS database_name
CHARACTER SET utf8mb4;
```

delete databse
```sql
DROP DATABASE IF EXISTS database_name;
```

# TABLES

delete / create table
```sql
DROP TABLE IF EXISTS
CREATE TABLE table (
    column INT PRIMARY KEY AUTO_INCREMENT,
    column_b CHAR NOT NULL,
    column_c INT NOT NULL DEFAULT 0,
    column_d VARCHAR(255) NOT NULL UNIQUE
) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;
```

modify table
```sql
ALTER TABLE table
    ADD column_a VARCHAR(50) NOT NULL AFTER column,
    ADD column_e INT NOT NULL UNIQUE,
    MODIFY COLUMN column_b VARCHAR(55) DEFAULT '',
    DROP column_d;
```

create relationships
```sql
DROP TABLE IF EXISTS
CREATE TABLE table (
    column_a INT PRIMARY KEY AUTO_INCREMENT,
    column_b CHAR NOT NULL,
    column_c INT NOT NULL DEFAULT 0,
    column_d VARCHAR(255) NOT NULL UNIQUE,
    FOREIGN KEY fk_table_other_table (column_a)
        REFERENCES other_table (column_a)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
);
```

alter / delete relationships
```sql
ALTER TABLE table
    DROP PRIMARY KEY,
    ADD PRIMARY KEY (other_id),
    DROP FOREIGN KEY fk_table_other_table,
    ADD FOREIGN KEY fk_table_that_table (column_b)
        REFERENCES other_table (column_a)
        ON UPDATE CASCADE
        ON DELETE NO ACTION;
```

# INDEXES

- Will increase query performances
- Will increase database size
- Will slow down write operation
- Should be reserved for performance critical queries
- Should be built based on the queries not the tables

Creating Index
```sql
CREATE INDEX idx_column ON table (column_name);
```

Creating Index on String columns
```sql
CREATE INDEX idx_column ON table (column_name(10));
```

Finding ideal index length
```sql
SELECT 
    COUNT(DISTINCT LEFT(column,1))
    COUNT(DISTINCT LEFT(column,2))
    COUNT(DISTINCT LEFT(column,5))
    COUNT(DISTINCT LEFT(column,10))
```
Pick the first length that returns a number close enough to the total number of rows. (90% is close enough)

Creating fulltext index
```sql
CREATE FULLTEXT INDEX idx_column_other_column ON table (column_name, other_column_name));
```

Using fulltext index
```sql
SELECT *
FROM table
WHERE MATCH (title, body) AGAINST ('word other_words');
```

Return revelevancy score
```sql
SELECT *, (MATCH (title, body) AGAINST ('word other_words')) AS 'score'
FROM table
WHERE MATCH (title, body) AGAINST ('word other_words');
```

Boolean mode
```sql
SELECT *
FROM table
WHERE MATCH (title, body) AGAINST ('word -other_words +must_word' IN BOOLEAN MODE);

SELECT *
FROM table
WHERE MATCH (title, body) AGAINST ('"exact phrase"' IN BOOLEAN MODE);
```

Creating composite index
```sql
CREATE INDEX idx_column_other_column ON table (column_name, other_column_name));
```

View Index
```sql
SHOW INDEXES IN table;
```

Delete Index
```sql
DROP INDEX idx_column_other_column ON table;
```

### Tips about index design
- Most frequently used columns first
- Higher cardinality column first
- Use index for sorting
- Use index for select
- Include more columns to create covering index
- Split "WHERE OR" clause in two UNION select clause to use indexes
- Do not create duplicate or redundant index

### Other Tips by Mosh Hamedani 

1. Smaller tables perform better. Don’t store the data you don’t need. Solve today’s problems, not tomorrow’s future problems that may never happen. 
2. Use the smallest data types possible. If you need to store people’s age, a TINYINT is sufficient. No need to use an INT. Saving a few bytes is not a big deal in a small table, but has a significant impact in a table with millions of records.
3. Every table must have a primary key.
4. Primary keys should be short. Prefer TINYINT to INT if you only need to store a hundred records.
5. Prefer numeric types to strings for primary keys. This makes looking up records by the primary key faster.
6. Avoid BLOBs. They increase the size of your database and have a negative impact on the performance. Store your files on disk if you can.
7. If a table has too many columns, consider splitting it into two related tables using a one-to-one relationship. This is called vertical partitioning. For example, you may have a customers table with columns for storing their address. If these columns don’t get read often, split the table into two tables (users and user_addresses).
8. In contrast, if you have several joins in your queries due to data fragmentation, you may want to consider denormalizing data. Denormalizing is the opposite of normalization. It involves duplicating a column from one table in another table (to reduce the number of joins) required.
9. Consider creating summary/cache tables for expensive queries. For example, if the query to fetch the list of forums and the number of posts in each forum is expensive, create a table called forums_summary that contains the list of forums and the number of posts in them. You can use events to regularly refresh the data in this table. You may also use triggers to update the counts every time there is a new post.
10. Full table scans are a major cause of slow queries. Use the EXPLAIN statement and look for queries with type = ALL. These are full table scans. Use indexes to optimize these queries.
11. When designing indexes, look at the columns in your WHERE clauses first. Those are the first candidates because they help narrow down the searches. Next, look at the columns used in the ORDER BY clauses. If they exist in the index, MySQL can scan your index to return ordered data without having to perform a sort operation (filesort). Finally, consider adding the columns in the SELECT clause to your indexes. This gives you a covering index that covers everything your query needs. MySQL doesn’t need to retrieve anything from your tables.
12. Prefer composite indexes to several single-column index.
13. The order of columns in indexes matter. Put the most frequently used columns and the columns with a higher cardinality first, but always take your queries into account.
14. Remove duplicate, redundant and unused indexes. Duplicate indexes are the indexes on the same set of columns with the same order. Redundant indexes are unnecessary indexes that can be replaced with the existing indexes. For example, if you have an index on columns (A, B) and create another index on column (A), the latter is redundant because the former index can help.
15. Don’t create a new index before analyzing the existing ones.
16. Isolate your columns in your queries so MySQL can use your indexes. 
17. Avoid SELECT *. Most of the time, selecting all columns ignores your indexes and returns unnecessary columns you may not need. This puts an extra load on your database server.
18. Return only the rows you need. Use the LIMIT clause to limit the number of rows returned.
19. Avoid LIKE expressions with a leading wildcard (eg LIKE ‘%name’).
20. If you have a slow query that uses the OR operator, consider chopping up the query into two queries that utilize separate indexes and combine them using the UNION operator.

