
# INSTALLATION 
- ### Postgresql Download Site
	[Download Postgresql](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads))
- ### Potential Error during installation for Windows
[COMSPEC environment variable error](https://stackoverflow.com/questions/72271531/postgresql-installation-error-the-environment-variable-comspec-does-not-seem-to)
- ### Two ways to use postgresql:
	- Using SQL Shell (psql) - command line interface
	- Using pgAdmin 4
- ### Check version:
```
SELECT version();
```

# DATATYPES
## Numeric
- #### smallint (2 bytes) 
- #### integer (4 bytes) - *typical choice for integer*
- #### bigint (8 bytes) 
- #### decimal (variable) - *user-specified precision,exact*
- #### numeric (variable) - *user-specified precision,exact*
- #### real (4 bytes) - *6 decimal digits precision*
- #### double precision (8 bytes) - *15 decimal digits precision*
- #### smallserial (2 bytes) - *autoincrement*
- #### serial (4 bytes) - *autoincrement*
- #### bigserial (8 bytes) - *autoincrement*


## Character Type
- #### character varying(n), varchar(n)
	*variable-length with limit*
- #### character (n), char(n)
	*fixed-length,  blank padded*
- #### text
	*variable unlimited length*


## Monetary Type
The _money_ type stores a currency amount with a fixed fractional precision
- #### money (8 bytes) - currency amount


## Date/Time Types
- #### timestamp [ (_`p`_) ]  [ without time zone ]
*both date and time, no timezone*
- #### timestamp [ (_`p`_) ] with time zone
*both date and time with timezone*
- #### date
*only date*
- #### time [ (_`p`_) ]  [ without time zone ]
*time of  day (no date)*
- #### time [ (_`p`_) ] with time zone
*time of day (no date) with timezone*


## Boolean Type
- #### boolean (1 byte)
*State of true or false*


## Enumerated Type
Data types that comprise a static, ordered set of values

``` sql
CREATE TYPE week AS ENUM ('Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun');
```

## Geometric Type
Representing 2 dimensional spatial object
- #### point (16 bytes)
	*point on a plane  (x,y)*
- #### line (32 bytes)
	*infinite line {A, B, C}*
- #### lseg(32 bytes)
	*Finite line segment ((x1,y1),(x2,y2))*
- #### box (32 bytes)
	*Rectangular box ((x1,y1),(x2,y2))*
- #### circle (24 bytes)
	*Circle <(x,y),r> (center point and radius)*
- #### polygon (40 + 16n bytes)
	*Polygon ((x1,y1),...)*
- #### path (16 + 16n bytes)
	*Open path [(x1,y1),...] *


## UUID Type
- #### uuid(128-bit)
*Universally Unique Identifiers (UUID) as defined by [RFC 4122](https://tools.ietf.org/html/rfc4122), ISO/IEC 9834-8:2005*


## XML Type
## JSON Type
## Array Type
Declaration of Array type
```sql
CREATE TABLE sal_emp (
    name            text,
    pay_by_quarter  integer[],
    schedule        text[][]
);
```
Or using SQL standard for array declaration
```sql
pay_by_quarter  integer ARRAY[4],
```
Array value input
	'{ _`val1`_ _`delim`_ _`val2`_ _`delim`_ ... }'
Examples:
```sql
INSERT INTO sal_emp
    VALUES ('Bill',
    '{10000, 10000, 10000, 10000}',
    '{{"meeting", "lunch"}, {"training", "presentation"}}');
```


## Composite Types
Declaration of composite type
```sql
CREATE TYPE inventory_item AS (
	name text,
	supplier_id integer,
	price numeric
);
```
Using composite type in creating table
```sql
CREATE TABLE on_hand AS (
	item inventory_item,
	count integer
)
```

## Object Identifier Types (OIDs)
## Pseudo Types

# DATABASE OPERATIONS
## Create database

Using command 
```
CREATE DATABASE dbname;
```
Or using OS l command
```
createdb [option...] [dbname [description]]
```
Example:
```
createdb -h localhost -p 5432 -U postgres testdb
password *******
```

## Select database

Using `\c`
```
\c dbname
```
Or using OS command
```
psql -h localhost -p 5432 -U postgress testdb
```

## Drop database

Syntax for dropping database
```
DROP DATABASE [ IF EXISTS ] dbname
```
Or using OS command
```
dropdb  [option...] dbname
```
Example
```
dropdb -h localhost -p 5432 -U postgress testdb
```

## Other useful psql commands
- `\l` : list all available database
- `\dt` : list all available table in current database
- `\d` : describe a table
- `\dn` : list all available schema in current database
- `\s` : command history
 
# Table Operations

## Create Table

Basic Syntax
```sql
CREATE TABLE table_name(
   column1 datatype,
   column2 datatype,
   column3 datatype,
   .....
   columnN datatype,
   PRIMARY KEY( one or more columns )
);
```
Example
```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```

## Drop table

Basic syntax
```sql
DROP TABLE tablename;
```

## Alter Table

The PostgreSQL **ALTER TABLE** command is used to add, delete or modify columns in an existing table. It can also used to add and drop constraints on an available table

- Add New Column
```sql
ALTER TABLE table_name ADD column_name datatype;
```
- Drop Column
```sql
ALTER TABLE table_name DROP COLUMN column_name;
```
- Changing datatypes
```sql
ALTER TABLE table_name ALTER COLUMN column_name TYPE datatype;
```
- Add constraints
```sql
-- Add unique constraint
ALTER TABLE table_name
ADD CONSTRAINT MyUniqueConstraint UNIQUE(column1, column2...);
-- Add check constraint
ALTER TABLE table_name
ADD CONSTRAINT MyUniqueConstraint CHECK (CONDITION);
-- Add Primary Key constraint
ALTER TABLE table_name
ADD CONSTRAINT MyPrimaryKey PRIMARY KEY (column1, column2...);
```
- Drop constraints
```sql
ALTER TABLE table_name
DROP CONSTRAINT MyUniqueConstraint;
ALTER TABLE table_name
DROP CONSTRAINT MyPrimaryKey;
```

# Query (Select)

## 1. Basic SELECT

- Basic syntax
```sql
SELECT column1, column2, columnN FROM table_name;
```
- Select all column
```sql
SELECT * FROM table_name;
```

## 2. Alias

Using Alias to rename a column or rename a table
- Rename Column
```sql
SELECT column_name AS alias_name
FROM table_name;
```
- Rename Table
```sql
SELECT column1, column2....
FROM table_name AS alias_name
```

## 3. Filtering query result 

- ### WHERE

**WHERE** is used to filter query result base on certain conditions.
```sql
SELECT select_list FROM table_name WHERE condition
```
The `condition` must evaluate to true, false, or unknown.
The query returns only rows that satisfy the `condition` in the `WHERE` clause.
`condition` is formed using comparison operators combined with logical operators

- ### Comparison operator
	
	- `=`  
	- `!=` or `<>` 
	- `>=` 
	- `<=` 
	
- ### Logical operator
	
	- **AND**
	- **OR**
	- **NOT**





- ### DISTINCT

The **DISTINCT** clause is used in the **SELECT** statement to remove duplicate rows from a result set.
```sql
SELECT DISTINCT column1_, column2, ...  
FROM table_name;
```

- ### IN

Using **IN** operator check if a value matches any value in a list of values.
```sql
value IN (value1,value2,...)
```
The list of values can also be a list of a result of a **SELECT** statement.
```sql
value IN (SELECT column_name FROM table_name);
```
Combine the **IN** operator with the **NOT**  operator to select rows whose values do not match the values in the list.
```sql
value NOT IN (value1,value2,...)
```

- ### BETWEEN

Use the **BETWEEN** operator to match a value against a range of values.
```sql
value BETWEEN low AND high;
```
**BETWEEN** is equivalent to 
```sql
value >= low and value <= high;
```
 Combine with **NOT** to check if value is out of range
```sql
value NOT BETWEEN low AND high;
```

- ### LIKE

**LIKE** operator is used to match text values against a pattern using wildcards.
Two types of wild card are used with **LIKE** :'' `%` 'and '`__`'
```sql
-- Finds any values that start with "a"
WHERE name LIKE 'a%'
-- Finds any values that end with "a"
WHERE name LIKE '%a'
-- Finds any values that have "a" in any position
WHERE name LIKE '%a%'
-- Finds any values that have "a" in the second position
WHERE name LIKE '_a%'
-- Finds any values that start with "a" and are at least 2 characters in length
WHERE name LIKE 'a_%'
-- Finds any values that start with "a" and are at least 3 characters in length
WHERE name LIKE 'a__%'
-- Finds any values that start with "a" and ends with "o"
WHERE name LIKE 'a%o'
```
- ### IS NULL



## 4. Organizing query result

- ### ORDER BY

**ORDER BY** is used to sort result set accordingly

```sql
SELECT select_list FROM table_name ORDER BY sort_expression1 [ASC | DESC], ... sort_expressionN [ASC | DESC];
```
**ASC** and **DESC** are optional, if there's neither of the two options, **ASC** will be the default mode.
When sorting multiple columns together, the first column will be sorted first, the following column will be sorted accordingly to the previous column.

- ### GROUP BY

**GROUP BY** clause divides the rows returned from the **SELECT** statement into groups
For each group, you can apply an aggregate function e.g., SUM() to calculate the sum of items or COUNT() to get the number of items in the groups.

```sql
SELECT column_1, column_2, ..., aggregate_function(column_3) FROM table_name 
GROUP BY column_1, column_2, ...;
```


- ### HAVING

**HAVING** clause specifies a filtering condition for a group or an aggregate.
**HAVING** clause is often used with the **GROUP BY** clause to filter groups or aggregates based on a specified condition.

```sql
SELECT column1, aggregate_function (column2) FROM table_name 
GROUP BY column1 
HAVING condition;
```


# Subquery

A subquery is a query within another PostgreSQL query and embedded within the **WHERE** clause.
A subquery is used to return data that will be used in the main query as a condition to further restrict the data to be retrieved.

- ### Subquery with the SELECT Clause

```sql
SELECT column_name [, column_name ]
FROM   table1 [, table2 ]
WHERE  column_name OPERATOR
      (SELECT column_name [, column_name ]
      FROM table1 [, table2 ]
      [WHERE])
```
Example
```sql
SELECT *
   FROM COMPANY
   WHERE ID IN (SELECT ID
      FROM COMPANY
      WHERE SALARY > 45000) ;
```

- ### Subquery with the INSERT Clause

```sql
INSERT INTO table_name [ (column1 [, column2 ]) ]
   SELECT [ *|column1 [, column2 ] ]
   FROM table1 [, table2 ]
   [ WHERE VALUE OPERATOR ]
```
Example
```sql
INSERT INTO COMPANY_BKP
   SELECT * FROM COMPANY
   WHERE ID IN (SELECT ID
      FROM COMPANY) ;
```


- ### Subquery with the UPDATE Clause

```sql
UPDATE table
SET column_name = new_value
[ WHERE OPERATOR [ VALUE ]
   (SELECT COLUMN_NAME
   FROM TABLE_NAME)
   [ WHERE)]
```
Example
```sql
UPDATE COMPANY
   SET SALARY = SALARY * 0.50
   WHERE AGE IN (SELECT AGE FROM COMPANY_BKP
      WHERE AGE >= 27 );
```


# With (Common table expression - CTEs)

A **common table expression**, or CTE, is a temporary named result set created from a simple **SELECT** statement that can be used in a subsequent **SELECT** statement. 
Each SQL CTE is like a **named query**, whose result is stored in a virtual table (a CTE) to be referenced later in the main query.

Syntax
```sql
WITH my_cte AS (
  SELECT a,b,c
  FROM T1
)
SELECT a,c
FROM my_cte
WHERE ....
```
**WITH** is the clause to create a CTE
`my_cte` is the name of the temporary query from the subquery

Example: `sale` table

|branch|date|seller|item|quantity|unit_price|
|---|---|---|---|---|---|
|Paris-1|2021-12-07|Charles|Headphones A2|1|80|
|London-1|2021-12-06|John|Cell Phone X2|2|120|
|London-2|2021-12-07|Mary|Headphones A1|1|60|
|Paris-1|2021-12-07|Charles|Battery Charger|1|50|
|London-2|2021-12-07|Mary|Cell Phone B2|2|90|
|London-1|2021-12-07|John|Headphones A0|5|75|
|London-1|2021-12-07|Sean|Cell Phone X1|2|100|

Obtain the price of the most expensive item using a CTE
```sql
WITH highest AS (
  SELECT
    branch,
    date,
    MAX(unit_price) AS highest_price
  FROM sales
  GROUP BY branch, date
)
SELECT
  sales.*,
  h.highest_price
FROM sales
JOIN highest h
  ON sales.branch = h.branch
    AND sales.date = h.date
```

Result

|branch|date|seller|item|quantity|unit_price|highest_ price|
|---|---|---|---|---|---|---|
|Paris-1|2021-12-07|Charles|Headphones A2|1|80|80|
|London-1|2021-12-06|John|Cell Phone X2|2|120|120|
|London-2|2021-12-07|Mary|Headphones A1|1|60|90|
|Paris-1|2021-12-07|Charles|Battery Charger|1|50|80|
|London-2|2021-12-07|Mary|Cell Phone B2|2|90|90|
|London-1|2021-12-07|John|Headphones A0|5|75|100|
|London-1|2021-12-07|Sean|Cell Phone X1|2|100|100|


# Views

**VIEW** is slightly similar to **CTE** as it create a temporary query to use in further queries
However, **CTE** is destroyed after the main query is finished while **VIEW** is still exist as an object until it is dropped.

- ### CREATE VIEW

```sql
CREATE [TEMP | TEMPORARY] VIEW view_name AS
SELECT column1, column2.....
FROM table_name
WHERE [condition]
```

- ### DROP VIEW 

```sql
DROP VIEW view_name;
```

# Data Manipulation (Insert, update, delete)

- ## INSERT

**INSERT** is used to create new records in a table.
```sql
INSERT INTO table_name (column_list) 
VALUES (value_list_1), (value_list_2), ... (value_list_n) 
[RETURNING * | output_expression];
```

**RETURNING** Clause is an option to return the information of the insert rows.
**INSERT** also produce an output message like
```
INSERT oid count
```
Where `count` is the number of rows successfully inserted, `oid` is the primary key generated by system for internal system use.



- ## UPDATE

**UPDATE** is used to modify existing records in a table.

```sql
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];
```

`condition` is used to filter which rows to be updated with new data.

- ## DELETE

**DELETE** is used to delete the existing records from a table.

```sql
DELETE FROM table_name
WHERE [condition];
```

`condition` is used to filter which rows to be deleted.

# Join

 **JOIN** is used to combine columns from one (self-join) or more tables based on the values of the common columns between related tables.
 The common columns are typically the **PRIMARY KEY** columns of the first table and **FOREIGN KEY** columns of the second table.
 There are 4 main type of **JOIN**: **INNER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN and FULL OUTER JOIN**

- ## INNER JOIN

The **INNER JOIN** is the most common type of join. It returns rows from joined tables that have matching rows. 
For example, if you have a table of employees and a table of departments, an inner join would return all the rows from the employee table where the employee's department ID is in the departments table.

```sql
SELECT table1.column1, table2.column2...
FROM table1
INNER JOIN table2
ON table1.common_filed = table2.common_field;
```


- ## LEFT OUTER JOIN

The **LEFT OUTER JOIN** returns all the rows from the left table, and the matched rows from the right table. 
For example, if you have a table of **customers** and a table of **orders**, a left outer join would return all the rows from the **customers** table, even if the customer doesn't have any orders. The rows from the **orders** table that don't have a matching **customer** will be returned with NULL values.

```sql
SELECT table1.column1, table2.column2...
FROM table1
LEFT OUTER JOIN table2
ON table1.common_filed = table2.common_field;
```

- ## RIGHT OUTER JOIN
The **RIGHT OUTER JOIN** returns all the rows from the right table, and the matched rows from the left table. 
```sql
SELECT table1.column1, table2.column2...
FROM table1
RIGHT OUTER JOIN table2
ON table1.common_filed = table2.common_field;
```

- ## FULL OUTER JOIN

The **FULL OUTER JOIN** returns all the rows from both tables, even if there are no matches

```sql
SELECT table1.column1, table2.column2...
FROM table1
FULL OUTER JOIN table2
ON table1.common_filed = table2.common_field;
```


# Union

The **UNION** operator combines result sets of two or more **SELECT** statements into a single result set. 
Requirements for **UNION**: 
- The number and the order of the columns in the select list of both **SELECT** must be equivalent.
- The data types must be compatible.

```sql
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]

UNION

SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition]
```

- **UNION** combines two queries but excludes duplicate rows.
- **UNION ALL**  combines two queries and retain duplicate rows.


# Constraints

**CONSTRAINTS** are rules to apply on data columns.
They prevents invalid data entering the database.
Constraints help maintaining the accuracy and the reliability of data in database

- ## NOT NULL

The NOT NULL constraint makes sure there is no NULL value in a column.
NULL value represents unknown data

```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```

- ## UNIQUE

The UNIQUE constraint makes sure there are no more than one record with the same value in a column.
There can be many UNIQUE columns in one table.


```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL UNIQUE,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```


- ## PRIMARY KEY

The PRIMARY KEY constraint uniquely identifies each record in a database table.
There can be many UNIQUE columns but there is only one PRIMARY KEY
PRIMARY KEY can be made up from 2 columns.
PRIMARY KEY can become FOREIGN KEY in a related table.

```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
```

- ## FOREIGN KEY

The FOREIGN KEY  constraint specifies that the values in a column (or a group of columns) must match the values appearing in some record of another related table.

```sql
CREATE TABLE DEPARTMENT(
   ID INT PRIMARY KEY      NOT NULL,
   DEPT           CHAR(50) NOT NULL,
   EMP_ID         INT      references COMPANY(ID)
);
```

The field `EMP_ID` is the FOREIGN KEY that references the `id` PRIMARY KEY field of table COMPANY

- ## CHECK

The CHECK constraint creates a condition to check the value being entered into a row. If the condition evaluates to false, the input row violates the constraint and is not entered into the table.

```sql
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL    CHECK(SALARY > 0)
);
```


# Indexes 

**Indexes** are used to sort data logically to improve the speed of searching and
sorting operations.
Primary key column has index by default.
Primary key data is always sorted; that’s just something the DBMS does internally

```sql
CREATE INDEX index_name
ON table_name (column1_name, column2_name);
```
# Transactions 

A **database transaction** is a single unit of work that consists of one or more operations. It makes sure that a series of SQL operations **execute completely or not at all.**

During a Transaction:
- If no error occurs, the entire set of statements is committed (written) to the database tables. 
- If an error does occur, then a rollback (undo) can occur to restore the database to a known and safe state.


Basic terms of a transaction:
- **Transaction** : A block of SQL statements.
- **Rollback** :  Undoing a certain SQL statements or a whole transaction.
- **Commit** : Writing unsaved SQL statements to the database tables
- **Savepoint** : A temporary checkpoint in a transaction set to which you can
issue a rollback 


Example
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
SAVEPOINT my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Bob';
-- oops ... forget that and use Wally's account
ROLLBACK TO my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Wally';
COMMIT;
```


# Lock

_Locks_ or _Exclusive Locks_ or _Write Locks_ prevent users from modifying a row or an entire table.

```sql
LOCK [ TABLE ]
name
 IN
lock_mode
```

# PL/pgSQL Procedure Language


## Language Basics
[PostgreSQL PL/pgSQL (postgresqltutorial.com)](https://www.postgresqltutorial.com/postgresql-plpgsql/)
[PostgreSQL: Documentation: 15: PostgreSQL 15.3 Documentation](https://www.postgresql.org/docs/15/index.html)


## User-defined Function
- #### Create Function

**PL/pgSQL** is a block-structured language. The complete text of a function body must be a _block_.

```sql
[ <<label>> ] 
[ declare 
	 declarations ] 
begin 
	statements; 
	... 
end [ label ];
```

Writing a function:
```sql
create [or replace] function function_name(param_list) 
returns return_type 
language plpgsql 
as 
<< label >>
$$ 
declare 
-- variable declaration 
begin 
-- logic 
end [label]
$$;
```
The function's main logic block is placed between `$$` signs

Example

```sql
CREATE FUNCTION somefunc() 
RETURNS integer 
LANGUAGE plpgsql
AS 
$$
<< outerblock >>
DECLARE
    quantity integer := 30;
BEGIN
    RAISE NOTICE 'Quantity here is %', quantity;  -- Prints 30
    quantity := 50;
    --
    -- Create a subblock
    --
    << innerblock >>
    DECLARE
        quantity integer := 80;
    BEGIN
        RAISE NOTICE 'Quantity here is %', quantity;  -- Prints 80
        RAISE NOTICE 'Outer quantity here is %', outerblock.quantity;  -- Prints 50
    END [ innerblock ];

    RAISE NOTICE 'Quantity here is %', quantity;  -- Prints 50

    RETURN quantity;
    
END [ outerblock ]
$$; 
```

To call a function 
```postgresql
select somefunc()
```

## Trigger

**Trigger** is  special user define function
**Trigger** is a callback function that is invoked automatically whenever an action or event happens (INSERT, UPDATE, DELETE, TRUNCATE)
Postgres provides two type of trigger:
- Row-level trigger 
- Statement-level trigger
If a trigger happen in a statement that modifies 20 rows, at row-level trigger, it would have been called 20 times

There are two steps to create a trigger
- Create a trigger function that return a trigger
- Bind the trigger to a certain table

```sql
------ Create trigger function -------
-- Syntax:
	-- CREATE FUNCTION trigger_function_name()
	-- RETURNS TRIGGER
	-- LANGUAGE plpgsql
	-- AS
	-- $$
	-- begin
	-- 	-- trigger logic
	-- end
	-- $$

-- Trigger function contains some local variables like 'OLD' and 'NEW' 
-- represent the states of the table before and after triggering event


------ Binding trigger to a table ------
-- Syntax:
	-- CREATE TRIGGER trigger_name 
	--    {BEFORE | AFTER} { event }
	--    ON table_name
	--    [FOR [EACH] { ROW | STATEMENT }]
	--        EXECUTE PROCEDURE trigger_function


------ EXAMPLE ------
---------------------

create table if not exists employees(
	id	INT GENERATED ALWAYS AS IDENTITY,
	first_name	VARCHAR(40) 	NOT NULL,
	last_name 	VARCHAR(40)		NOT NULL,
	PRIMARY KEY (id)
);

-- Audit table keep change log everytime employee last name change
CREATE TABLE IF NOT EXISTS employee_audits (
   id INT GENERATED ALWAYS AS IDENTITY,
   employee_id INT NOT NULL,
   old_last_name VARCHAR(40) NOT NULL,
   new_last_name VARCHAR(40) NOT NULL,
   changed_on TIMESTAMP(6) NOT NULL
);


-- create a trigger function 
create or replace function log_last_name_change()
returns trigger
language plpgsql
as
$$
begin
	if OLD.last_name <> NEW.last_name then
		insert into employee_audits(employee_id, old_last_name, new_last_name, changed_on) values (OLD.id, OLD.last_name, NEW.last_name, now());
	end if;
	
	return new;
end
$$;

-- Bind trigger to a table (employee)
create trigger last_name_change
before update 
on employees
for each row
execute procedure log_last_name_change();
```

# REFERENCES
- [PostgreSQL Tutorial (tutorialspoint.com)](https://www.tutorialspoint.com/postgresql/index.htm)
- [PostgreSQL PL/pgSQL (postgresqltutorial.com)](https://www.postgresqltutorial.com/postgresql-plpgsql/)
- [PostgreSQL: Documentation: 15: PostgreSQL 15.3 Documentation](https://www.postgresql.org/docs/15/index.html)
 








