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


Exercises:

1. Create a view named **list_of_customers**, it should contain the following columns:
 customer id, customer full name,  address,  zip code, phone,city, country,  status = active if active or inactive if null and store id

2. Create a view named **film_details**, it should contain the following columns:
film id,  title, description,  category,  price,  length,  rating, actors  - as a string of all the actors separated by comma. Hint use GROUP_CONCAT

3. Create view **sales_by_film_category**, it should return 'category' and 'total_rental' columns.

4. Create a view called **actor_information** where it should return, actor id, first name, last name and the amount of films he/she acted on.

5. Analyze view **actor_info**, explain the entire query and specially how the sub query works. Be very specific, take some time and decompose each part and give an explanation for each. 

6. Materialized views, write a description, why they are used, alternatives, DBMS were they exist, etc. 