## MySQL Full-Text Search

MySQL supports text searching by using the LIKE operator and regular expression. However, when the text column is large and the number of rows in a table is increased, using these methods has some limitations:

* Performance: MySQL has to scan the whole table to find the exact text based on a pattern in the LIKE  statement or pattern in the regular expressions.
* Flexible search: with the LIKE operator and regular expression searches, it is difficult to have a flexible search query e.g., to find product whose description contains car  but not classic.
* Relevance ranking: there is no way to specify which row in the result set is more relevant to the search terms.
* Because of these limitations, MySQL extended a very nice feature so-called full-text search. Technically, MySQL creates an index from the words of the enabled full-text search columns and performs searches on this index. MySQL uses a sophisticated algorithm to determine the rows matched against the search query.

##### The following are some important features of MySQL full-text search:

* Native SQL-like interface: you use the SQL-like statement to use the full-text search.
* Fully dynamic index: MySQL automatically updates the index of text column whenever the data of that column changes.
* Moderate index size: it doesn’t take much memory to store the index.
* Last but not least, it is fast to search based on complex search queries.
* Notice that not all storage engines support the full-text search feature. In MySQL version 5.6 or later, only MyISAM and InnoDB storage engines support full-text search.


## Defining FULLTEXT Indexes for MySQL Full-Text Searching

Before performing a full-text search in a column of a table, you must index its data. MySQL will recreate the full-text index whenever the data of the column changes. In MySQL, the full-text index is a kind of index that has a name FULLTEXT.

MySQL supports indexing and re-indexing data automatically for a full-text search enabled column. MySQL version 5.6 or later allows you to define a full-text index for a column whose data type is CHAR, VARCHAR or TEXT in MyISAM or InnoDB table type. Notice that MySQL supported full-text index in the InnoDB tables since version 5.6.

MySQL allows you to define the FULLTEXT index by using the CREATE TABLE statement when you create the table or ALTER TABLE or CREATE INDEX statement for the existing tables.

#### Defining FULLTEXT index using CREATE TABLE statement

Typically, you define the FULLTEXT index for a column when you create a new table using the CREATE TABLE statement as follows:

```sql
CREATE TABLE table_name(
	column1 data_type,	
        column2 data_type,
        column3 data_type,
	…
PRIMARY_KEY(key_column),
FULLTEXT (column1,column2,..)
);
```

To create the FULLTEXT index, you place a list of comma-separated columns in parentheses after the FULLTEXT keyword.

The following statement creates a new table named posts that has a FULLTEXT index that includes the post_content column.

```
CREATE TABLE posts (
  id int(4) NOT NULL AUTO_INCREMENT,
  title varchar(255) NOT NULL,
  post_content text,
  PRIMARY KEY (id),
  FULLTEXT KEY post_content (post_content)
);
```

### Defining FULLTEXT index for existing tables

In case you already have existing tables and want to define full-text indexes, you can use the ALTER TABLE statement or CREATE INDEX statement.

### Defining FULLTEXT index using ALTER TABLE statement

The following syntax defines a FULLTEXT index using the ALTER TABLE statement:

```sql
ALTER TABLE  table_name  
ADD FULLTEXT(column_name1, column_name2,…)
```

You put the table_name is the ADD FULLTEXT clause that defines a FULLTEXT index for one or more columns.

For example, you can define a FULLTEXT index for the productDescription and productLine columns in the products table of the sample database as follows:

```sql
ALTER TABLE products  
ADD FULLTEXT(productDescription,productLine)
```

### Defining FULLTEXT index using CREATE INDEX statement

You can also use the CREATE INDEX statement to create a FULLTEXT index for existing tables. See the following syntax:

```sql
CREATE FULLTEXT INDEX index_name
ON table_name(idx_column_name,...)
```

The following statement creates a FULLTEXT index for the addressLine1 and addressLine2 columns of the offices table.

```sql
CREATE FULLTEXT INDEX address
ON offices(addressLine1,addressLine2)
```

Notice that for a table which has many rows, it is faster to load the data into the table that has no FULLTEXT index first and then create the FULLTEXT index, than loading a large amount of data into a table that has an existing FULLTEXT index.

### Removing full-text search columns

To remove a FULLTEXT index, you just delete the index using the ALTER TABLE … DROP INDEX statement. For example, the following statement removes the address FULLTEXT index in the offices table:

```sql
ALTER TABLE offices
DROP INDEX address;
```

### MySQL natural language full-text search example

We will use the products table in the sample database for the demonstration.


First, you need to enable full-text search in the productLine  column of the products  table using the ALTER TABLE ADD FULLTEXT  statement:

```sql
ALTER TABLE products 
ADD FULLTEXT(productline);
```

Second, you can search for products whose product lines contain the term Classic . You use the MATCH()  and AGAINST()  functions as the following query:

```
SELECT productName, productline
FROM products
WHERE MATCH(productline) AGAINST('Classic');
```

To search for product whose product line contains Classic or Vintage term, you can perform the following query:

```sql
SELECT productName, productline
FROM products
WHERE MATCH(productline) AGAINST('Classic,Vintage');
```

The AGAINST()  function uses IN NATURAL LANGUAGE MODE  search modifier by default therefore you can omit it in the query. There are other search modifiers e.g.,  IN BOOLEAN MODE   for Boolean text searches.

You can explicitly use the IN NATURAL LANGUAGE MODE  search modifier in your query as follows:

```sql
SELECT productName, productline
FROM products
WHERE MATCH(productline) 
AGAINST('Classic,Vintage' IN NATURAL LANGUAGE MODE)
```

By default, MySQL performs searches in the case-insensitive fashion. However, you can instruct MySQL to perform case-sensitive searches using binary collation for indexed columns.

### Sort the result set by relevance

A very important feature of full-text search is how MySQL ranks the rows in the result set based on their relevance. When the MATCH()  function is used in the WHERE clause, MySQL returns the rows that are more relevant first.

The following example shows you how MySQL sorts the result set by the relevance.

First, you enable the full-text search feature for the  productName column of the products table.

```sql
ALTER TABLE products 
ADD FULLTEXT(productName);
```

Second, you search for products whose names contain  Ford  and/or  1932 using the following query:

```sql
SELECT productName, productline
FROM products
WHERE MATCH(productName) AGAINST('1932,Ford')
```

The products, whose names contain both 1932  and Ford are returned first and then the products whose names contains the only Ford keyword.
