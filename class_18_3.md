# Listing Stored Procedures in a MySQL Database

MySQL provides us with several useful statements that help manage stored procedures more effectively. Those statements include listing stored procedures and showing the stored procedure’s source code.

## Displaying characteristics of stored procedures

To display characteristics of a stored procedure, you use the  SHOW PROCEDURE STATUS statement as follows:

```sql
SHOW PROCEDURE STATUS [LIKE 'pattern' | WHERE expr];
```

The SHOW PROCEDURE STATUS statement is a MySQL extension to SQL standard. This statement gives you the stored procedure’s characteristics including database, stored procedure name, type, creator, etc.

You can use LIKE or WHERE clause to filter out the stored procedure based on various criteria.

To list all stored procedures of the databases that you have the privilege to access, you use the  SHOW PROCEDURE STATUS statement as follows:

```sql
SHOW PROCEDURE STATUS;
```

If you want to show just stored procedure in a particular database, you can use the WHERE clause in the  SHOW PROCEDURE STATUS statement:

```sql
SHOW PROCEDURE STATUS WHERE db = 'classicmodels';
```

If you want to show stored procedures that have a particular pattern e.g., its name contains product, you can use LIKE operator as the following command:

```sql
SHOW PROCEDURE STATUS WHERE name LIKE '%product%'
```

## Displaying stored procedure’s source code

To display source code of a particular stored procedure, you use the  SHOW CREATE PROCEDURE statement as follows:

```sql
SHOW CREATE PROCEDURE stored_procedure_name
```

You specify the name of the stored procedure after the  SHOW CREATE PROCEDURE keywords. For example, to display the code of the GetAllProducts stored procedure, you use the following statement:

```sql
SHOW CREATE PROCEDURE GetAllProducts
```

# MySQL Error Handling in Stored Procedures

When an error occurs inside a stored procedure, it is important to handle it appropriately, such as continuing or exiting the current code block’s execution, and issuing a meaningful error message.

MySQL provides an easy way to define handlers that handle from general conditions such as warnings or exceptions to specific conditions e.g., specific error codes.

## Declaring a handler

To declare a handler, you use the  DECLARE HANDLER statement as follows:

```sql
DECLARE action HANDLER FOR condition_value statement;
```

If a condition whose value matches the  condition_value , MySQL will execute the statement and continue or exit the current code block based on the action .

The action accepts one of the following values:

* CONTINUE :  the execution of the enclosing code block ( BEGIN … END ) continues.
* EXIT : the execution of the enclosing code block, where the handler is declared, terminates.

The  condition_value specifies a particular condition or a class of conditions that activates the handler. The  condition_value accepts one of the following values:

* A MySQL error code.
* A standard SQLSTATE value. Or it can be an SQLWARNING , NOTFOUND or SQLEXCEPTION condition, which is shorthand for the class of SQLSTATE values. The NOTFOUND condition is used for a cursor or  SELECT INTO variable_list statement.
* A named condition associated with either a MySQL error code or SQLSTATE value.

The statement could be a simple statement or a compound statement enclosing by the BEGIN and END keywords.

## MySQL error handling examples

Let’s look into several examples of declaring handlers.

The following handler means that if an error occurs, set the value of the  has_error variable to 1 and continue the execution.

```sql
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET has_error = 1;
```

The following is another handler which means that in case any error occurs, rollback the previous operation, issue an error message, and exit the current code block. If you declare it inside the BEGIN END block of a stored procedure, it will terminate stored procedure immediately.

```sql
DECLARE EXIT HANDLER FOR SQLEXCEPTION
BEGIN
ROLLBACK;
SELECT 'An error has occurred, operation rollbacked and the stored procedure was terminated';
END;
```

The following handler means that if there are no more rows to fetch, in case of a cursor or SELECT INTO statement, set the value of the  no_row_found variable to 1 and continue execution.

```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND SET no_row_found = 1;
```

The following handler means that if a duplicate key error occurs, MySQL error 1062 is issued. It issues an error message and continues execution.

```sql
DECLARE CONTINUE HANDLER FOR 1062
SELECT 'Error, duplicate key occurred';
```

## MySQL handler example in stored procedures

First, we create a new table named  article_tags for the demonstration:

```sql
CREATE TABLE article_tags(
    article_id INT,
    tag_id     INT,
    PRIMARY KEY(article_id,tag_id)
);
```

The  article_tags table stores the relationships between articles and tags. Each article may have many tags and vice versa. For the sake of simplicity, we don’t create articles and tags tables, as well as the foreign keys in the  article_tags table.

Next, we create a stored procedure that inserts article id and tag id into the article_tags table:

```sql
DELIMITER $$

CREATE PROCEDURE insert_article_tags(IN article_id INT, IN tag_id INT)
BEGIN

	DECLARE CONTINUE HANDLER FOR 1062
	SELECT CONCAT('duplicate keys (',article_id,',',tag_id,') found') AS msg;

	-- insert a new record into article_tags
	INSERT INTO article_tags(article_id,tag_id)
	VALUES(article_id,tag_id);

	-- return tag count for the article
	SELECT COUNT(*) FROM article_tags;
END
```

Then, we add tag id 1, 2 and 3 for the article 1 by calling the insert_article_tags  stored procedure as follows:

```sql
CALL insert_article_tags(1,1);
CALL insert_article_tags(1,2);
CALL insert_article_tags(1,3);
```

After that, we try to insert a duplicate key to check if the handler is really invoked.

```sql
CALL insert_article_tags(1,3);
```

We got an error message. However, because we declared the handler as a CONTINUE handler, the stored procedure continued the execution. As the result, we got the tag count for the article as well.

![mysql-error-handling](/uploads/4301cec178394e3b2fcb8bb575496c04/mysql-error-handling.jpg)

If we change the CONTINUE in the handler declaration to EXIT , we will get an error message only.

```sql
DELIMITER $$

CREATE PROCEDURE insert_article_tags_2(IN article_id INT, IN tag_id INT)
BEGIN

	DECLARE EXIT HANDLER FOR SQLEXCEPTION 
	SELECT 'SQLException invoked';

	DECLARE EXIT HANDLER FOR 1062 
        SELECT 'MySQL error code 1062 invoked';

	DECLARE EXIT HANDLER FOR SQLSTATE '23000'
	SELECT 'SQLSTATE 23000 invoked';

	-- insert a new record into article_tags
	INSERT INTO article_tags(article_id,tag_id)
   	VALUES(article_id,tag_id);

	-- return tag count for the article
	SELECT COUNT(*) FROM article_tags;
END
```

Finally, we can try to add a duplicate key to see the effect.

```sql
CALL insert_article_tags_2(1,3);
```

![mysql-error-handling-duplicate-keys](/uploads/8ff6e46dfb214df49031229cd85c87a2/mysql-error-handling-duplicate-keys.jpg)

## MySQL handler precedence

In case there are multiple handlers that are eligible for handling an error, MySQL will call the most specific handler to handle the error first.

An error always maps to one MySQL error code because in MySQL it is the most specific. An SQLSTATE may map to many MySQL error codes therefore it is less specific. An SQLEXCPETION or an SQLWARNING is the shorthand for a class of SQLSTATES values so it is the most generic.

Based on the handler precedence’s rules,  MySQL error code handler, SQLSTATE handler and SQLEXCEPTION takes the first, second and third precedence.

Suppose we declare three handlers in the  insert_article_tags_3 stored procedure as follows:

```sql
DELIMITER $$

CREATE PROCEDURE insert_article_tags_3(IN article_id INT, IN tag_id INT)
BEGIN

	DECLARE EXIT HANDLER FOR 1062 SELECT 'Duplicate keys error encountered';
	DECLARE EXIT HANDLER FOR SQLEXCEPTION SELECT 'SQLException encountered';
	DECLARE EXIT HANDLER FOR SQLSTATE '23000' SELECT 'SQLSTATE 23000';

	-- insert a new record into article_tags
	INSERT INTO article_tags(article_id,tag_id)
	VALUES(article_id,tag_id);

	-- return tag count for the article
	SELECT COUNT(*) FROM article_tags;
END
```

We try to insert a duplicate key into the article_tags table by calling the stored procedure:

```sql
CALL insert_article_tags_3(1,3);
```

As you see the MySQL error code handler is called.

![MySQL-handler-precedence](/uploads/e8ab9b0a630627902bc4e83c13e05654/MySQL-handler-precedence.jpg)

Using named error condition

Let’s start with an error handler declaration.

```sql
DECLARE EXIT HANDLER FOR 1051 SELECT 'Please create table abc first';
SELECT * FROM abc;
```

What does the number 1051 really mean? Imagine you have a big stored procedure polluted with those numbers all over places; it will become a nightmare to maintain the code.

Fortunately, MySQL provides us with the DECLARE CONDITION statement that declares a named error condition, which associates with a condition.

The syntax of the DECLARE CONDITION statement is as follows:

```sql
DECLARE condition_name CONDITION FOR condition_value;
```

The condition_value  can be a MySQL error code such as 1015 or a SQLSTATE value. The condition_value is represented by the condition_name .

After declaration, we can refer to condition_name  instead of condition_value .

So we can rewrite the code above as follows:

```sql
DECLARE table_not_found CONDITION for 1051;
DECLARE EXIT HANDLER FOR  table_not_found SELECT 'Please create table abc first';
SELECT * FROM abc;
```

This code is obviously more readable than the previous one.

# Raising Error Conditions with MySQL SIGNAL / RESIGNAL Statements


## MySQL SIGNAL statement

You use the SIGNAL  statement to return an error or warning condition to the caller from a stored program e.g., stored procedure, stored function, trigger or event. The SIGNAL  statement provides you with control over which information for returning such as value and messageSQLSTATE.

The following illustrates syntax of the SIGNAL statement:

```sql
SIGNAL SQLSTATE | condition_name;
SET condition_information_item_name_1 = value_1,
    condition_information_item_name_1 = value_2, etc;
```

Following the SIGNAL keyword is a SQLSTATE value or a condition name declared by the  DECLARE CONDITION statement. Notice that the SIGNAL statement must always specify a SQLSTATE value or a named condition that defined with an  SQLSTATE value.

To provide the caller with information, you use the SET clause. If you want to return multiple condition information item names with values, you need to separate each name/value pair by a comma.

The  condition_information_item_name can be MESSAGE_TEXT, MYSQL_ERRORNO, CURSOR_NAME , etc.

The following stored procedure adds an order line item into an existing sales order. It issues an error message if the order number does not exist.

```sql
DELIMITER $$

CREATE PROCEDURE AddOrderItem(
		         in orderNo int,
			 in productCode varchar(45),
			 in qty int, 
                         in price double, 
                         in lineNo int )
BEGIN
	DECLARE C INT;

	SELECT COUNT(orderNumber) INTO C
	FROM orders 
	WHERE orderNumber = orderNo;

	-- check if orderNumber exists
	IF(C != 1) THEN 
		SIGNAL SQLSTATE '45000'
			SET MESSAGE_TEXT = 'Order No not found in orders table';
	END IF;
	-- more code below
	-- ...
END
```

First, it counts the orders with the input order number that we pass to the stored procedure.

Second, if the number of order is not 1, it raises an error with  SQLSTATE 45000 along with an error message saying that order number does not exist in the orders table.

Notice that 45000 is a generic SQLSTATE value that illustrates an unhandled user-defined exception.

If we call the stored procedure  AddOrderItem() and pass a nonexistent order number, we will get an error message.

```sql
CALL AddOrderItem(10,'S10_1678',1,95.7,1);
```

![mysql-signal](/uploads/5bce133e2edd45ac460d7bf79d74753d/mysql-signal.jpg)

## MySQL RESIGNAL statement

Besides the SIGNAL  statement, MySQL also provides the RESIGNAL  statement used to raise a warning or error condition.

The RESIGNAL  statement is similar to SIGNAL  statement in term of functionality and syntax, except that:

* You must use the RESIGNAL  statement within an error or warning handler, otherwise, you will get an error message saying that “RESIGNAL when handler is not active”. Notice that you can use SIGNAL  statement anywhere inside a stored procedure.
* You can omit all attributes of the RESIGNAL statement, even the SQLSTATE value.

If you use the RESIGNAL  statement alone, all attributes are the same as the ones passed to the condition handler.

The following stored procedure changes the error message before issuing it to the caller.

``sql
DELIMITER $$

CREATE PROCEDURE Divide(IN numerator INT, IN denominator INT, OUT result double)
BEGIN
	DECLARE division_by_zero CONDITION FOR SQLSTATE '22012';

	DECLARE CONTINUE HANDLER FOR division_by_zero 
	RESIGNAL SET MESSAGE_TEXT = 'Division by zero / Denominator cannot be zero';
	-- 
	IF denominator = 0 THEN
		SIGNAL division_by_zero;
	ELSE
		SET result := numerator / denominator;
	END IF;
END
```

Let’s call the  Divide() stored procedure.

```sql
CALL Divide(10,0,@result);
```

![mysql-resignal](/uploads/cc8b7cc452c5fb878299a7e7b31a9ce7/mysql-resignal.jpg)