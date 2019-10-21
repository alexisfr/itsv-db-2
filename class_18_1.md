# MySQL Stored Procedures That Return Multiple Values


MySQL stored function returns only one value. To develop stored programs that return multiple values, you need to use stored procedures with INOUT or OUT parameters.

## Stored procedures that return multiple values example

Let’s take a look at the orders table in the sample database.

The following stored procedure accepts customer number and returns the total number of orders that was shipped, canceled, resolved, and disputed.

```sql
DELIMITER $$

CREATE PROCEDURE get_order_by_cust(
	IN cust_no INT,
	OUT shipped INT,
	OUT canceled INT,
	OUT resolved INT,
	OUT disputed INT)
BEGIN
		-- shipped
		SELECT
            count(*) INTO shipped
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Shipped';

		-- canceled
		SELECT
            count(*) INTO canceled
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Canceled';

		-- resolved
		SELECT
            count(*) INTO resolved
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Resolved';

		-- disputed
		SELECT
            count(*) INTO disputed
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Disputed';

END
```

In addition to the IN parameter, the stored procedure takes 4 additional OUT parameters: shipped, canceled, resolved, and disputed. Inside the stored procedure, you use a SELECT statement with the COUNT function to get the corresponding total of orders based on the order’s status and assign it to the respective parameter.

To use the get_order_by_cust stored procedure, you pass customer number and four user-defined variables to get the out values.

After executing the stored procedure, you use the SELECT statement to output the variable values.

```sql
CALL get_order_by_cust(141,@shipped,@canceled,@resolved,@disputed);
SELECT @shipped,@canceled,@resolved,@disputed;
```

# MySQL IF Statement


The MySQL IF statement allows you to execute a set of SQL statements based on a certain condition or value of an expression. To form an expression in MySQL, you can combine literals, variables, operators, and even functions. An expression can return one of three values TRUE FALSE, or NULL.

Note that there is an IF function that is different from the IF statement specified in this tutorial.

## MySQL IF statement syntax

The following illustrates the syntax of the IF statement:

```sql
IF expression THEN 
   statements;
END IF;
```

If the expression evaluates to TRUE , then the statements will be executed, otherwise, the control is passed to the next statement following the END IF.

## MySQL IF ELSE statement

In case you want to execute statements when the expression evaluates to FALSE , you use the IF ELSE statement as follows:

```sql
IF expression THEN
   statements;
ELSE
   else-statements;
END IF;
```

## MySQL IF ELSEIF ELSE statement

If you want to execute statements conditionally based on multiple expressions, you use the IF ELSEIF ELSE statement as follows:

```sql
IF expression THEN
   statements;
ELSEIF elseif-expression THEN
   elseif-statements;
...
ELSE
   else-statements;
END IF;
```

If the expression evaluates to TRUE , the statements in the IF branch executes. If the expression evaluates to FALSE , MySQL will check the elseif-expression and execute the elseif-statements in the ELSEIF branch if the elseif_expression evaluates to TRUE .

The IF statement may have multiple ELSEIF branches to check multiple expressions. If no expression evaluates to TRUE, the else-statements in the ELSE branch will execute.


## MySQL IF statement examples

The following example illustrates how to use the IF ESLEIF ELSE statement. The GetCustomerLevel()   stored procedure accepts two parameters customer number and customer level.

First, it gets the credit limit from the customers table.

Then, based on the credit limit, it determines the customer level: PLATINUM , GOLD , and SILVER .

The parameter p_customerlevel stores the level of the customer and is used by the calling program.

```sql
DELIMITER $$

CREATE PROCEDURE GetCustomerLevel(
    in  p_customerNumber int(11), 
    out p_customerLevel  varchar(10))
BEGIN
    DECLARE creditlim double;

    SELECT creditlimit INTO creditlim
    FROM customers
    WHERE customerNumber = p_customerNumber;

    IF creditlim > 50000 THEN
	SET p_customerLevel = 'PLATINUM';
    ELSEIF (creditlim <= 50000 AND creditlim >= 10000) THEN
        SET p_customerLevel = 'GOLD';
    ELSEIF creditlim < 10000 THEN
        SET p_customerLevel = 'SILVER';
    END IF;

END$$
```

The following flowchart demonstrates the logic of determining customer level.

![mysql-if-statement-flow-chart](/images/mysql-if-statement-flow-chart.png)


# MySQL CASE Statement


Besides the IF statement, MySQL provides an alternative conditional statement called CASE. The MySQL CASE statement makes the code more readable and efficient.

There are two forms of the CASE statements: simple and searched CASE statements.

## Simple CASE statement

Let’s take a look at the syntax of the simple CASE statement:

```sql
CASE  case_expression
   WHEN when_expression_1 THEN commands
   WHEN when_expression_2 THEN commands
   ...
   ELSE commands
END CASE;
```

You use the simple CASE statement to check the value of an expression against a set of unique values.

The  case_expression can be any valid expression. We compare the value of the case_expression with  when_expression  in each WHEN clause e.g., when_expression_1 , when_expression_2 , etc. If the value of the case_expression and when_expression_n are equal, the  commands  in the corresponding WHEN branch executes.

In case none of the when_expression in the WHEN clause matches the value of the case_expression , the commands in the ELSE clause will execute. The ELSE  clause is optional. If you omit the ELSE clause and no match found, MySQL will raise an error.

The following example illustrates how to use the simple CASE statement:

```sql
DELIMITER $$

CREATE PROCEDURE GetCustomerShipping(
		in  p_customerNumber int(11), 
		out p_shiping        varchar(50))
BEGIN
    DECLARE customerCountry varchar(50);

    SELECT country INTO customerCountry
	FROM customers
	WHERE customerNumber = p_customerNumber;

    CASE customerCountry
		WHEN  'USA' THEN
		   SET p_shiping = '2-day Shipping';
		WHEN 'Canada' THEN
		   SET p_shiping = '3-day Shipping';
		ELSE
		   SET p_shiping = '5-day Shipping';
	END CASE;

END$$
```

How the stored procedure works.

* The GetCustomerShipping stored procedure accepts customer number as an IN parameter and returns shipping period based on the country of the customer.
* Inside the stored procedure, first we get the country of the customer based on the input customer number. Then we use the simple CASE statement to compare the country of the customer to determine the shipping period. If the customer locates in USA , the shipping period is 2-day shipping . If the customer is in Canada , the shipping period is 3-day shipping The customers from other countries have 5-day shipping .

The following flowchart demonstrates the logic of determining shipping period.

![mysql-case-statement](/images/mysql-case-statement.png)

The following is the test script for the stored procedure above:

```sql
SET @customerNo = 112;

SELECT country into @country
FROM customers
WHERE customernumber = @customerNo;

CALL GetCustomerShipping(@customerNo,@shipping);

SELECT @customerNo AS Customer,
       @country    AS Country,
       @shipping   AS Shipping;
```

## Searched CASE statement

The simple CASE statement only allows you match a value of an expression against a set of distinct values. In order to perform more complex matches such as ranges, you use the searched CASE statement. The searched CASE statement is equivalent to the IF  statement, however, its construct is much more readable.

The following illustrates the syntax of the searched CASE statement:

```sql
CASE
    WHEN condition_1 THEN commands
    WHEN condition_2 THEN commands
    ...
    ELSE commands
END CASE;
```

MySQL evaluates each condition in the WHEN clause until it finds a condition whose value is TRUE , then corresponding commands in the THEN clause will execute.

If no condition is TRUE  , the command in the ELSE clause will execute. If you don’t specify the ELSE clause and no condition is TRUE , MySQL will issue an error message.

MySQL does not allow you to have empty commands in the THEN or ELSE clause. If you don’t want to handle the logic in the ELSE clause while preventing MySQL raise an error, you can put an empty BEGIN END  block in the ELSE clause.

The following example demonstrates using searched CASE statement to find customer level SILVER , GOLD or PLATINUM based on customer’s credit limit.

```sql
DELIMITER $$

CREATE PROCEDURE GetCustomerLevel(
	in  p_customerNumber int(11), 
	out p_customerLevel  varchar(10))
BEGIN
    DECLARE creditlim double;

    SELECT creditlimit INTO creditlim
	FROM customers
	WHERE customerNumber = p_customerNumber;

    CASE  
		WHEN creditlim > 50000 THEN 
		   SET p_customerLevel = 'PLATINUM';
		WHEN (creditlim <= 50000 AND creditlim >= 10000) THEN
		   SET p_customerLevel = 'GOLD';
		WHEN creditlim < 10000 THEN
		   SET p_customerLevel = 'SILVER';
	END CASE;

END$$
```

If the credit limit is

* greater than 50K, then the customer is the PLATINUM customer
* less than 50K and greater than 10K, then the customer is the GOLD customer
* less than 10K, then the customer is the SILVER customer.

We can test our stored procedure by executing the following test script:

```sql
CALL GetCustomerLevel(112,@level);
SELECT @level AS 'Customer Level';
```

### Hints for Choosing Between IF and CASE Statements


MySQL provides both IF and CASE statements to enable you to execute a block of SQL code based on certain conditions, which is known as flow control. So what statement should you use? For the most developers, choosing between IF and CASE is just a matter of personal preference. However, when you decide to use IF or CASE ,  you should take the following points into the consideration:

* A simple CASE statement is more readable than the IF statement when you compare a single expression against a range of unique values. In addition, the simple CASE statement is more efficient than the IF statement.
* When you check complex expressions based on multiple values, the IF statement is easier to understand.
* If you choose to use the CASE statement, you have to make sure that at least one of the CASE condition is matched. Otherwise, you need to define an error handler to catch the error. Recall that you don’t have to do this with the IF statement.
* In most organization, there is always something called development guidelines document that provides developers with naming convention and guidelines on programming style. You should refer to this document and follow the development practices.
* In some situations, mixing between IF and CASE make your stored procedure more readable and efficient.