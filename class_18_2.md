# MySQL Loop in Stored Procedures

MySQL provides loop statements that allow you to execute a block of SQL code repeatedly based on a condition. There are three loop statements in MySQL: WHILE, REPEAT and LOOP.

We will examine each loop statement in more detail in the following sections.

## WHILE loop

The syntax of the WHILE statement is as follows:

```sql
WHILE expression DO
   statements
END WHILE
```

The WHILE loop checks the expressionat the beginning of each iteration. If the expressionevaluates to TRUE, MySQL will execute statementsbetween WHILEand END WHILE until the expressionevaluates to FALSE. The WHILE loop is called pretest loop because it checks the expression before the statements execute. 

Here is an example of using the WHILE loop statement in a stored procedure:

```sql
 DELIMITER $$
 DROP PROCEDURE IF EXISTS test_mysql_while_loop$$
 CREATE PROCEDURE test_mysql_while_loop()
	BEGIN
		DECLARE x  INT;
		DECLARE str  VARCHAR(255);
		
		SET x = 1;
		SET str =  '';
		
		WHILE x  <= 5 DO
			SET  str = CONCAT(str,x,',');
			SET  x = x + 1; 
		END WHILE;
		
		SELECT str;
	END$$
DELIMITER ;
```

In the test_mysql_while_loop stored procedure above:

* First, we build str string repeatedly until the value of the x variable is greater than 5.
* Then, we display the final string using the SELECT statement.

Notice that if we don’t initialize the x variable, its default value is NULL. Therefore, the condition in the WHILE loop statement is always TRUE and you will have an indefinite loop, which is not expected.

Let’s test the test_mysql_while_loopstored procedure:

```sql
CALL test_mysql_while_loop();
```


## REPEAT loop

The syntax of the REPEAT loop statement is as follows:

```sql
REPEAT
 statements;
UNTIL expression
END REPEAT
```

First, MySQL executes the statements, and then it evaluates the expression. If the expression evaluates to FALSE, MySQL executes the statements repeatedly until the expression evaluates to TRUE.

Because the REPEAT loop statement checks the expression after the execution of statements therefore the REPEAT loop statement is also known as the post-test loop.

We can rewrite the test_mysql_while_loop stored procedure that uses WHILE loop statement above using the REPEAT loop statement:

```sql
 DELIMITER $$
 DROP PROCEDURE IF EXISTS mysql_test_repeat_loop$$
 CREATE PROCEDURE mysql_test_repeat_loop()
	BEGIN
		DECLARE x INT;
		DECLARE str VARCHAR(255);
        
		SET x = 1;
        SET str =  '';
        
		REPEAT
			SET  str = CONCAT(str,x,',');
			SET  x = x + 1; 
        UNTIL x  > 5
        END REPEAT;

        SELECT str;
	END$$
 DELIMITER ;
```

It is noted that there is no semicolon (;) in the UNTIL expression.

```sql
CALL mysql_test_repeat_loop();
````

## LOOP, LEAVE and ITERATE statements

There are two statements that allow you to control the loop:

* The LEAVE statement allows you to exit the loop immediately without waiting for checking the condition. The LEAVE statement works like the  break statement in other languages such as PHP, C/C++, Java, etc.
* The ITERATE statement allows you to skip the entire code under it and start a new iteration. The ITERATE statement is similar to the continue statement in PHP, C/C++, Java, etc.

MySQL also gives you a LOOPstatement that executes a block of code repeatedly with an additional flexibility of using a loop label.

The following is an example of using the LOOP loop statement.

```sql
CREATE PROCEDURE test_mysql_loop()
	BEGIN
		DECLARE x  INT;
        DECLARE str  VARCHAR(255);
        
		SET x = 1;
        SET str =  '';
        
		loop_label:  LOOP
			IF  x > 10 THEN 
				LEAVE  loop_label;
			END  IF;
            
			SET  x = x + 1;
			IF  (x mod 2) THEN
				ITERATE  loop_label;
			ELSE
                SET  str = CONCAT(str,x,',');
			END  IF;
         END LOOP;    

         SELECT str;

	END;
```
 
* The stored procedure only constructs a string with even numbers e.g., 2, 4, 6, etc.
* We put a loop_label  loop label before the LOOPstatement.
* If the value of  x is greater than 10, the loop is terminated because of the LEAVE statement.
* If the value of the x is an odd number, the ITERATE statement ignores everything below it and starts a new iteration.
* If the value of the x is an even number, the block in the ELSE statement will build the string with even numbers.

# MySQL Cursor


## Introduction to MySQL cursor

To handle a result set inside a stored procedure, you use a cursor. A cursor allows you to iterate a set of rows returned by a query and process each row accordingly.

MySQL cursor is read-only, non-scrollable and asensitive.

* Read only: you cannot update data in the underlying table through the cursor.
* Non-scrollable: you can only fetch rows in the order determined by the SELECT statement. You cannot fetch rows in the reversed order. In addition, you cannot skip rows or jump to a specific row in the result set.
* Asensitive: there are two kinds of cursors: asensitive cursor and insensitive cursor. An asensitive cursor points to the actual data, whereas an insensitive cursor uses a temporary copy of the data. An asensitive cursor performs faster than an insensitive cursor because it does not have to make a temporary copy of data. However, any change that made to the data from other connections will affect the data that is being used by an asensitive cursor, therefore, it is safer if you don’t update the data that is being used by an asensitive cursor. MySQL cursor is asensitive.

You can use MySQL cursors in stored procedures, stored functions, and triggers.

## Working with MySQL cursor

First, you have to declare a cursor by using the DECLARE statement:

```sql
DECLARE cursor_name CURSOR FOR SELECT_statement;
```

The cursor declaration must be after any variable declaration. If you declare a cursor before variables declaration, MySQL will issue an error. A cursor must always be associated with a SELECT statement.

Next, you open the cursor by using the OPEN statement. The OPEN statement initializes the result set for the cursor, therefore, you must call the OPEN statement before fetching rows from the result set.

```sql
OPEN cursor_name;
```

Then, you use the FETCH statement to retrieve the next row pointed by the cursor and move the cursor to the next row in the result set.

```sql
FETCH cursor_name INTO variables list;
```

After that, you can check to see if there is any row available before fetching it.

Finally, you call the CLOSE statement to deactivate the cursor and release the memory associated with it as follows:

```sql
CLOSE cursor_name;
```

When the cursor is no longer used, you should close it.

When working with MySQL cursor, you must also declare a NOT FOUND handler to handle the situation when the cursor could not find any row. Because each time you call the FETCH statement, the cursor attempts to read the next row in the result set. When the cursor reaches the end of the result set, it will not be able to get the data, and a condition is raised. The handler is used to handle this condition.

To declare a NOT FOUND handler, you use the following syntax:

```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;
```

Where finished is a variable to indicate that the cursor has reached the end of the result set. Notice that the handler declaration must appear after variable and cursor declaration inside the stored procedures.

The following diagram illustrates how MySQL cursor works.

![mysql-cursor](/images/mysql-cursor.png)

## MySQL Cursor Example

We are going to develop a stored procedure that builds an email list of all employees in the employees table in the MySQL sample database.

First, we declare some variables, a cursor for looping over the emails of employees, and a NOT FOUND handler:

```sql
DECLARE finished INTEGER DEFAULT 0;
DECLARE email varchar(255) DEFAULT "";

-- declare cursor for employee email
DEClARE email_cursor CURSOR FOR 
	SELECT email FROM employees;

-- declare NOT FOUND handler
DECLARE CONTINUE HANDLER 
FOR NOT FOUND SET finished = 1;
```

Next, we open the email_cursor by using the OPEN statement:

```sql
OPEN email_cursor;
```

Then, we iterate the email list, and concatenate all emails where each email is separated by a semicolon(;):

```sql
get_email: LOOP
	FETCH email_cursor INTO v_email;
	IF v_finished = 1 THEN 
		LEAVE get_email;
	END IF;
	-- build email list
	SET email_list = CONCAT(v_email,";",email_list);
END LOOP get_email;
```

After that, inside the loop we used the v_finished variable to check if there is any email in the list to terminate the loop.

Finally, we close the cursor using the CLOSE statement:
```sql
CLOSE email_cursor;
```

The build_email_list stored procedure is as follows:

```sql
DELIMITER $$

CREATE PROCEDURE build_email_list (INOUT email_list varchar(4000))
BEGIN

	DECLARE v_finished INTEGER DEFAULT 0;
        DECLARE v_email varchar(100) DEFAULT "";

	-- declare cursor for employee email
	DEClARE email_cursor CURSOR FOR 
		SELECT email FROM employees;

	-- declare NOT FOUND handler
	DECLARE CONTINUE HANDLER 
        FOR NOT FOUND SET v_finished = 1;

	OPEN email_cursor;

	get_email: LOOP

		FETCH email_cursor INTO v_email;

		IF v_finished = 1 THEN 
			LEAVE get_email;
		END IF;

		-- build email list
		SET email_list = CONCAT(v_email,";",email_list);

	END LOOP get_email;

	CLOSE email_cursor;

END$$
```

You can test the build_email_list stored procedure using the following script:

```sql
SET @email_list = "";
CALL build_email_list(@email_list);
SELECT @email_list;
```
