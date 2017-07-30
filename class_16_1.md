### SQL Constraints
Constraints are the rules enforced on data columns on a table. These are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the database.

Constraints can either be column level or table level. Column level constraints are applied only to one column whereas, table level constraints are applied to the entire table.

#### NOT NULL Constraint − Ensures that a column cannot have NULL value.

By default, a column can hold NULL values. If you do not want a column to have a NULL value, then you need to define such a constraint on this column specifying that NULL is now not allowed for that column.

A NULL is not the same as no data, rather, it represents unknown data.

#### Example
For example, the following SQL query creates a new table called CUSTOMERS and adds five columns, three of which, are ID NAME and AGE, In this we specify not to accept NULLs −

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```

If CUSTOMERS table has already been created, then to add a NOT NULL constraint to the SALARY column, you would write a query like the one that is shown in the following code block.

```sql
ALTER TABLE CUSTOMERS
   MODIFY SALARY  DECIMAL (18, 2) NOT NULL;
```
#### DEFAULT Constraint − Provides a default value for a column when none is specified.

The DEFAULT constraint provides a default value to a column when the INSERT INTO statement does not provide a specific value.

##### Example
For example, the following SQL creates a new table called CUSTOMERS and adds five columns. Here, the SALARY column is set to 5000.00 by default, so in case the INSERT INTO statement does not provide a value for this column, then by default this column would be set to 5000.00.

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2) DEFAULT 5000.00,       
   PRIMARY KEY (ID)
);
```
If the CUSTOMERS table has already been created, then to add a DEFAULT constraint to the SALARY column, you would write a query like the one which is shown in the code block below.

```sql
ALTER TABLE CUSTOMERS
MODIFY SALARY  DECIMAL (18, 2) DEFAULT 5000.00; 
```
##### Drop Default Constraint
To drop a DEFAULT constraint, use the following SQL query.
```sql
ALTER TABLE CUSTOMERS
   ALTER COLUMN SALARY DROP DEFAULT;
```
#### UNIQUE Constraint − Ensures that all values in a column are different.

The UNIQUE Constraint prevents two records from having identical values in a column. In the CUSTOMERS table, for example, you might want to prevent two or more people from having an identical age.

##### Example
For example, the following SQL query creates a new table called CUSTOMERS and adds five columns. Here, the AGE column is set to UNIQUE, so that you cannot have two records with the same age.

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL UNIQUE,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```
If the CUSTOMERS table has already been created, then to add a UNIQUE constraint to the AGE column. You would write a statement like the query that is given in the code block below.

```sql
ALTER TABLE CUSTOMERS
   MODIFY AGE INT NOT NULL UNIQUE;
```
You can also use the following syntax, which supports naming the constraint in multiple columns as well.
```sql
ALTER TABLE CUSTOMERS
   ADD CONSTRAINT myUniqueConstraint UNIQUE(AGE, SALARY);
````
##### DROP a UNIQUE Constraint
To drop a UNIQUE constraint, use the following SQL query.

```sql
ALTER TABLE CUSTOMERS
   DROP CONSTRAINT myUniqueConstraint;
```
If you are using MySQL, then you can use the following syntax −
```sql
ALTER TABLE CUSTOMERS
   DROP INDEX myUniqueConstraint;
```
#### PRIMARY Key − Uniquely identifies each row/record in a database table.

A primary key is a field in a table which uniquely identifies each row/record in a database table. Primary keys must contain unique values. A primary key column cannot have NULL values.

A table can have only one primary key, which may consist of single or multiple fields. When multiple fields are used as a primary key, they are called a composite key.

If a table has a primary key defined on any field(s), then you cannot have two records having the same value of that field(s).

Note − You would use these concepts while creating database tables.

##### Create Primary Key
Here is the syntax to define the ID attribute as a primary key in a CUSTOMERS table.

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```
To create a PRIMARY KEY constraint on the "ID" column when the CUSTOMERS table already exists, use the following SQL syntax −

ALTER TABLE CUSTOMER ADD PRIMARY KEY (ID);
NOTE − If you use the ALTER TABLE statement to add a primary key, the primary key column(s) should have already been declared to not contain NULL values (when the table was first created).

For defining a PRIMARY KEY constraint on multiple columns, use the SQL syntax given below.

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),        
   PRIMARY KEY (ID, NAME)
);
```
To create a PRIMARY KEY constraint on the "ID" and "NAMES" columns when CUSTOMERS table already exists, use the following SQL syntax.

```sql
ALTER TABLE CUSTOMERS 
   ADD CONSTRAINT PK_CUSTID PRIMARY KEY (ID, NAME);
```
##### Delete Primary Key
You can clear the primary key constraints from the table with the syntax given below.
```sql
ALTER TABLE CUSTOMERS DROP PRIMARY KEY ;
```
####  FOREIGN Key − Uniquely identifies a row/record in any of the given database table.

A foreign key is a key used to link two tables together. This is sometimes also called as a referencing key.

A Foreign Key is a column or a combination of columns whose values match a Primary Key in a different table.

The relationship between 2 tables matches the Primary Key in one of the tables with a Foreign Key in the second table.

If a table has a primary key defined on any field(s), then you cannot have two records having the same value of that field(s).

##### Example
Consider the structure of the following two tables.

CUSTOMERS table

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```

ORDERS table

```sql
CREATE TABLE ORDERS (
   ID          INT        NOT NULL,
   DATE        DATETIME, 
   CUSTOMER_ID INT references CUSTOMERS(ID),
   AMOUNT     double,
   PRIMARY KEY (ID)
);
```
If the ORDERS table has already been created and the foreign key has not yet been set, the use the syntax for specifying a foreign key by altering a table.
```sql
ALTER TABLE ORDERS 
   ADD FOREIGN KEY (Customer_ID) REFERENCES CUSTOMERS (ID);
```
##### DROP a FOREIGN KEY Constraint
To drop a FOREIGN KEY constraint, use the following SQL syntax.

```sql
ALTER TABLE ORDERS
   DROP FOREIGN KEY;
```
#### CHECK Constraint − The CHECK constraint ensures that all the values in a column satisfies certain conditions.

The CHECK Constraint enables a condition to check the value being entered into a record. If the condition evaluates to false, the record violates the constraint and isn't entered the table.

##### Example
For example, the following program creates a new table called CUSTOMERS and adds five columns. Here, we add a CHECK with AGE column, so that you cannot have any CUSTOMER who is below 18 years.

```sql
CREATE TABLE CUSTOMERS(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL CHECK (AGE >= 18),
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```
If the CUSTOMERS table has already been created, then to add a CHECK constraint to AGE column, you would write a statement like the one given below.

```sql
ALTER TABLE CUSTOMERS
   MODIFY AGE INT NOT NULL CHECK (AGE >= 18 );
```
You can also use the following syntax, which supports naming the constraint in multiple columns as well −

```sql
ALTER TABLE CUSTOMERS
   ADD CONSTRAINT myCheckConstraint CHECK(AGE >= 18);
```
##### DROP a CHECK Constraint
To drop a CHECK constraint, use the following SQL syntax. This syntax does not work with MySQL.

```sql
ALTER TABLE CUSTOMERS
   DROP CONSTRAINT myCheckConstraint;
```