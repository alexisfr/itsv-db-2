A VIEW is not a physical table, but rather, it is in essence a virtual table created by a query joining one or more tables.

### Create VIEW

Syntax

The syntax for the CREATE VIEW statement in MySQL is:
```sql
CREATE [OR REPLACE] VIEW view_name AS
  SELECT columns
  FROM tables
  [WHERE conditions];
```
#### OR REPLACE
Optional. If you do not specify this clause and the VIEW already exists, the CREATE VIEW statement will return an error.
#### view_name
The name of the VIEW that you wish to create in MySQL.
#### WHERE conditions
Optional. The conditions that must be met for the records to be included in the VIEW.
Example

Here is an example of how to use the CREATE VIEW statement to create a view in MySQL:

```sql
CREATE VIEW hardware_suppliers AS
  SELECT supplier_id, supplier_name
  FROM suppliers
  WHERE category_type = 'Hardware';
```

This CREATE VIEW example would create a virtual table based on the result set of the SELECT statement. You can now query the MySQL VIEW as follows:

```sql
SELECT *
FROM hardware_suppliers;
```

### Update VIEW

You can modify the definition of a VIEW in MySQL without dropping it by using the ALTER VIEW statement.

Syntax

The syntax for the ALTER VIEW statement in MySQL is:
```sql
ALTER VIEW view_name AS
  SELECT columns
  FROM table
  WHERE conditions;
```

Example

Here is an example of how you would use the ALTER VIEW statement in MySQL:

```sql
ALTER VIEW hardware_suppliers AS
  SELECT supplier_id, supplier_name, address, city
  FROM suppliers
  WHERE category_type = 'Hardware';
```

This ALTER VIEW example in MySQL would update the definition of the VIEW called hardware_suppliers without dropping it. In this example, we are adding the address and city columns to the VIEW.

### Drop VIEW

Once a VIEW has been created in MySQL, you can drop it with the DROP VIEW statement.

Syntax

The syntax for the DROP VIEW statement in MySQL is:
```sql
DROP VIEW [IF EXISTS] view_name;
view_name
```

The name of the view that you wish to drop.
#### IF EXISTS
Optional. If you do not specify this clause and the VIEW does not exist, the DROP VIEW statement will return an error.
Example

Here is an example of how to use the DROP VIEW statement in MySQL:
```sql
DROP VIEW hardware_suppliers;
```

This DROP VIEW example would drop/delete the MySQL VIEW called hardware_suppliers.