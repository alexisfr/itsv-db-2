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