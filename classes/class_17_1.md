## Indexes

A database index, or just index, helps speed up the retrieval of data from tables. When you query data from a table, first MySQL checks if the indexes exist, then MySQL uses the indexes to select exact physical corresponding rows of the table instead of scanning the whole table.

A database index is similar to an index of a book. If you want to find a topic, you look up in the index first, and then you open the page that has the topic without scanning the whole book.

It is highly recommended that you create indices on columns of a table from which you often query the data. Notice that all primary key columns are in the primary index of  the table automatically.

If index helps speed up the querying data, why don’t we use indexes for all columns? If you create an index for every column, MySQL has to build and maintain the index table. Whenever a change is made to the rows of the table, MySQL has to rebuild the index, which takes time as well as decreases the performance of the database server.

### Creating MySQL Index

You often create indexes when you create tables. MySQL automatically adds any column that is declared as **PRIMARY KEY**, **KEY** , **UNIQUE** or **INDEX** to the index. In addition, you can add indexes to the tables that already have data.

In order to create indexes, you use the **CREATE INDEX**  statement. The following illustrates the syntax of the **CREATE INDEX** statement:
```sql
CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
  [ USING BTREE | HASH ]
  ON table_name
    (index_col1 [(length)] [ASC | DESC], 
     index_col2 [(length)] [ASC | DESC],
     ...
     index_col_n [(length)] [ASC | DESC]);
```
##### UNIQUE
Optional. The UNIQUE modifier indicates that the combination of values in the indexed columns must be unique.
##### FULLTEXT
Optional. The FULLTEXT modifier indexes the entire column and does not allow prefixing. InnoDB and MyISAM tables support this option.
##### SPATIAL
Optional. The SPATIAL modifier indexes the entire column and does not allow indexed columns to contain NULL values. InnoDB (starting in MySQL 5.7) and MyISAM tables support this option.
##### index_name
The name to assign to the index.
##### table_name
The name of the table in which to create the index.
index_col1, index_col2, ... index_col_n
The columns to use in the index.
##### length
Optional. If specified, only a prefix of the column is indexed not the entire column. For non-binary string columns, this value is the given number of characters of the column to index. For binary string columns, this value is the given number of bytes of the column to index.

##### ASC
Optional. The index is sorted in ascending order for that column.
##### DESC
Optional. The index is sorted in descending order for that column.

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