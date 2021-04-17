### DDL Review

Data Definition Language consist of commands to create and drop database objects, like tables, indexes, views, etc.

### Data types

String Datatypes

|  Syntax | Maximum Size                                     | Explanation                                                                                                                   |
|------------------|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| CHAR(size)       | Maximum size of 255 characters.                  | Where size is the number of characters to store. Fixed-length strings. Space padded on right to equal size characters.        |
| VARCHAR(size)    | Maximum size of 65,535 characters.                  | Where size is the number of characters to store. Variable-length string.                                                      |
| TINYTEXT(size)   | Maximum size of 255 characters.                  | Where size is the number of characters to store.                                                                              |
| TEXT(size)       | Maximum size of 65,535 characters.               | Where size is the number of characters to store.                                                                              |
| MEDIUMTEXT(size) | Maximum size of 16,777,215 characters.           | Where size is the number of characters to store.                                                                              |
| LONGTEXT(size)   | Maximum size of 4GB or 4,294,967,295 characters. | Where size is the number of characters to store.                                                                              |
| BINARY(size)     | Maximum size of 255 characters.                  | Where size is the number of binary characters to store. Fixed-length strings. Space padded on right to equal size characters. |
| VARBINARY(size)  | Maximum size of 255 characters.                  | Where size is the number of characters to store. Variable-length string.                                                      |

Numeric Datatypes

|  Syntax      | Maximum Size                                                                                                                | Explanation                                                                                                                                                                       |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BIT(m)                | Where m goes from 1 to 64. Default is 1.                                                  | Numbers represented in bits e.g. BIT(5) type with value b'00111' is the numeric value 7.                                                                                                                        |
| TINYINT(m)            | Signed values range from -128 to 127. Unsigned values range from 0 to 255.                                                  | Very small integer value.                                                                                                                                                         |
| SMALLINT(m)           | Signed values range from -32768 to 32767. Unsigned values range from 0 to 65535.                                            | Small integer value.                                                                                                                                                              |
| MEDIUMINT(m)          | Signed values range from -8388608 to 8388607. Unsigned values range from 0 to 16777215.                                     | Medium integer value.                                                                                                                                                             |
| INT(m)                | Signed values range from -2147483648 to 2147483647. Unsigned values range from 0 to 4294967295.                             | Standard integer value.                                                                                                                                                           |
| INTEGER(m)            | Signed values range from -2147483648 to 2147483647. Unsigned values range from 0 to 4294967295.                             | Standard integer value. This is a synonym for the INT datatype.                                                                                                                   |
| BIGINT(m)             | Signed values range from -9223372036854775808 to 9223372036854775807. Unsigned values range from 0 to 18446744073709551615. | Big integer value.                                                                                                                                                                |
| DECIMAL(m,d)          |                                                                                                                             | Unpacked fixed point number. m defaults to 10, if not specified.  d defaults to 0, if not specified. Where m is the total digits and d is the number of digits after the decimal. |
| DEC(m,d)              |                                                                                                                             | This is a synonym for the DECIMAL datatype.                                                                                                                                       |
| NUMERIC(m,d)          |                                                                                                                             | This is a synonym for the DECIMAL datatype.                                                                                                                                       |
| FIXED(m,d)            |                                                                                                                             | Unpacked fixed-point number. Where m is the total digits and d is the number of digits after the decimal. (Introduced in MySQL 4.1). This is a synonym for the DECIMAL datatype.  |
| FLOAT(m,d)            |                                                                                                                             | Single precision floating point number. Where m is the total digits and d is the number of digits after the decimal.                                                              |
| DOUBLE(m,d)           |                                                                                                                             | Double precision floating point number. Where m is the total digits and d is the number of digits after the decimal.                                                              |
| DOUBLE PRECISION(m,d) |                                                                                                                             | This is a synonym for the DOUBLE datatype.                                                                                                                                        |
| REAL(m,d)             |                                                                                                                             | This is a synonym for the DOUBLE datatype.                                                                                                                                        |
| FLOAT(p)              |                                                                                                                             | Floating point number. Where p is the precision.                                                                                                                                  |
| BOOL                  |                                                                                                                             | Treated as a boolean data type where a value of 0 is considered to be FALSE and any other value is considered to be TRUE. Synonym for TINYINT(1)                                  |
| BOOLEAN               |                                                                                                                             | This is a synonym for the BOOL datatype.                                                                                                                                          |

Date/Time Datatypes

|  Syntax | Maximum Size                                                          | Explanation                       |
|------------------|-----------------------------------------------------------------------|-----------------------------------|
| DATE             | Values range from 1000-01-01 to 9999-12-31.                           | Displayed as YYYY-MM-DD.          |
| DATETIME         | Values range from 1000-01-01 00:00:00 to 9999-12-31 23:59:59.         | Displayed as YYYY-MM-DD HH:MM:SS. |
| TIMESTAMP(m)     | Values range from 1970-01-01 00:00:01 UTC to 2038-01-19 03:14:07 UTC. | Displayed as YYYY-MM-DD HH:MM:SS. |
| TIME             | Values range from -838:59:59 to 838:59:59.                            | Displayed as HH:MM:SS.            |
| YEAR[(2 or 4)]      | Year value as 2 digits or 4 digits.                                   | Default is 4 digits.              |

Large Object (LOB) Datatypes

| Syntax | Maximum Size                                     | Explanation                                                                                        |
|------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
| TINYBLOB         | Maximum size of 255 bytes.                       |                                                                                                    |
| BLOB(size)       | Maximum size of 65,535 bytes.                    | Where size is the number of characters to store (size is optional and was introduced in MySQL 4.1) |
| MEDIUMBLOB       | Maximum size of 16,777,215 bytes.                |                                                                                                    |
| LONGTEXT         | Maximum size of 4GB or 4,294,967,295 characters. |                                                                                                    |

Enum Type

|  Syntax | Maximum Size                                     | Explanation                                                                                        |
|------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
| ENUM             |                                                  | An ENUM is a string object with a value chosen from a list of permitted values that are enumerated explicitly in the column specification at table creation time |

### Table creation

```sql
CREATE [ TEMPORARY ] TABLE [IF NOT EXISTS] table_name
( 
  column1 datatype [ NULL | NOT NULL ]
                   [ DEFAULT default_value ]
                   [ AUTO_INCREMENT ]
                   [ UNIQUE KEY | PRIMARY KEY ]
                   [ COMMENT 'string' ],
  ...
   | [CONSTRAINT [constraint_name]] PRIMARY KEY [ USING BTREE | HASH ] (index_col_name, ...)
  ...
  | [CONSTRAINT [constraint_name]] 
        FOREIGN KEY index_name (index_col_name, ...)
        REFERENCES another_table_name (index_col_name, ...)
        [ MATCH FULL | MATCH PARTIAL | MATCH SIMPLE ]
        [ ON DELETE { RESTRICT | CASCADE | SET NULL | NO ACTION } ]
        [ ON UPDATE { RESTRICT | CASCADE | SET NULL | NO ACTION } ]
);
```
Example

```sql
CREATE TABLE contacts
( contact_id INT(11) NOT NULL AUTO_INCREMENT,
  last_name VARCHAR(30) NOT NULL,
  first_name VARCHAR(25),
  birthday DATE,
  CONSTRAINT contacts_pk PRIMARY KEY (contact_id)
);
```

### Altering Tables
#### Add Primary Keys

```sql
ALTER TABLE table_name
  ADD CONSTRAINT [ constraint_name ]
    PRIMARY KEY [ USING BTREE | HASH ] (column1, column2, ... column_n)
```
Example

```sql
ALTER TABLE contacts
  ADD CONSTRAINT contacts_pk
    PRIMARY KEY (last_name, first_name);
```

#### Add column
```sql
ALTER TABLE table_name
  ADD new_column_name column_definition
    [ FIRST | AFTER column_name ];
```

Example

```sql
ALTER TABLE contacts
  ADD last_name varchar(40) NOT NULL
    AFTER contact_id;
```

#### Modify column definition (datatype)

```sql
ALTER TABLE table_name
  MODIFY column_name column_definition
    [ FIRST | AFTER column_name ];
```

Example

```sql
ALTER TABLE contacts
  MODIFY last_name varchar(50) NULL;
```

#### Drop column

```sql
ALTER TABLE table_name
  DROP COLUMN column_name;
```

#### Rename column

```sql
ALTER TABLE table_name
  CHANGE COLUMN old_name new_name 
    column_definition
    [ FIRST | AFTER column_name ]
```

Example

```sql
ALTER TABLE contacts
  CHANGE COLUMN contact_type ctype
    varchar(20) NOT NULL;
```

#### Rename table

```sql
ALTER TABLE table_name
  RENAME TO new_table_name;
```

Example

```sql
ALTER TABLE contacts
  RENAME TO people;
```

#### Add Foreign Key 

```sql
ALTER TABLE contacts
  ADD
[CONSTRAINT [symbol]] FOREIGN KEY
    [index_name] (index_col_name, ...)
    REFERENCES tbl_name (index_col_name,...)
    [ON DELETE reference_option]
    [ON UPDATE reference_option]

--reference_option:
--    RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT
```

Example

```sql
CREATE TABLE products
( product_name VARCHAR(50) NOT NULL,
  location VARCHAR(50) NOT NULL,
  category VARCHAR(25),
  CONSTRAINT products_pk PRIMARY KEY (product_name, location)
);

CREATE TABLE inventory
( inventory_id INT PRIMARY KEY,
  product_name VARCHAR(50) NOT NULL,
  location VARCHAR(50) NOT NULL,
  quantity INT,
  min_level INT,
  max_level INT
);

ALTER TABLE inventory ADD 
  CONSTRAINT fk_inventory_products
    FOREIGN KEY (product_name, location)
    REFERENCES products (product_name, location);
```
