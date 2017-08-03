## Indexes

A database index, or just index, helps speed up the retrieval of data from tables. When you query data from a table, first MySQL checks if the indexes exist, then MySQL uses the indexes to select exact physical corresponding rows of the table instead of scanning the whole table.

A database index is similar to an index of a book. If you want to find a topic, you look up in the index first, and then you open the page that has the topic without scanning the whole book.

It is highly recommended that you create indices on columns of a table from which you often query the data. Notice that all primary key columns are in the primary index of  the table automatically.

If index helps speed up the querying data, why don’t we use indexes for all columns? If you create an index for every column, MySQL has to build and maintain the index table. Whenever a change is made to the rows of the table, MySQL has to rebuild the index, which takes time as well as decreases the performance of the database server.

### Creating MySQL Index

You often create indexes when you create tables. MySQL automatically adds any column that is declared as **PRIMARY KEY**, **KEY** , **UNIQUE** or **INDEX** to the index. In addition, you can add indexes to the tables that already have data.

In order to create indexes, you use the **CREATE INDEX**  statement. The following illustrates the syntax of the **CREATE INDEX** statement:

```sql
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
USING [BTREE | HASH | RTREE] 
ON table_name (column_name [(length)] [ASC | DESC],...)
```

First, you specify the index based on the table type or storage engine:


1.  For the **UNIQUE** index, MySQL creates a constraint that all values in the index must be unique. Duplicate NULL values are allowed in all storage engines except for BDB.

2. The **FULLTEXT** index is supported only by MyISAM storage engine and only accepted on a column whose has data type is **CHAR**, **VARCHAR** or **TEXT**.

3. The **SPATIAL** index supports spatial column and is available on MyISAM storage engine. In addition, the column value must not be NULL.

### Example of creating index in MySQL

In the sample database, you can add  postalCode column of  the customers table to the index by using the CREATE INDEX  statement as follows:

```sql
CREATE INDEX postalCode ON customers(postalCode);
```

### Removing Indexes

Besides creating an index, you can also remove index by using the DROP INDEX  statement. Interestingly, the DROP INDEX  statement is also mapped to ALTER TABLE statement. The following is the syntax of removing the index:

```sql
DROP INDEX index_name ON table_name
```

For example, if you want to drop index postalCode of the employees  table,  which we have created above, you can execute the following query:

```sql
DROP INDEX postalCode ON customers
```