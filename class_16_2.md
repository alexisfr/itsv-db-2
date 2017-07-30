## Triggers

A SQL trigger is a set of  SQL statements stored in the database catalog. A SQL trigger is executed or fired whenever an event associated with a table occurs e.g.,  insert, update or delete.

A SQL trigger is a special type of stored procedure. It is special because it is not called directly like a stored procedure. The main difference between a trigger and a stored procedure is that a trigger is called automatically when a data modification event is made against a table whereas a stored procedure must be called explicitly.

It is important to understand SQL trigger’s advantages and disadvantages so that you can use it appropriately. In the following sections, we will discuss the advantages and disadvantages of using SQL triggers.

### Advantages of using SQL triggers

* SQL triggers provide an alternative way to check the integrity of data.
* SQL triggers can catch errors in business logic in the database layer.
* SQL triggers provide an alternative way to run scheduled tasks. By using SQL triggers, you don’t have to wait to run the scheduled tasks because the triggers are invoked  automatically before or after a change  is made to the data in the tables.
* SQL triggers are very useful to audit the changes of data in tables.

### Disadvantages of using SQL triggers

* SQL triggers only can provide an extended validation and they cannot replace all the validations. Some simple validations have to be done in the application layer. For example, you can validate user’s inputs in the client side by using JavaScript or in the server side using server-side scripting languages such as JSP, PHP, ASP.NET, Perl, etc.
* SQL triggers are invoked and executed invisible from the client applications, therefore, it is difficult to figure out what happen in the database layer.
* SQL triggers may increase the overhead of the database server.


## MySQL triggers

In MySQL, a trigger is a set of SQL statements that is invoked automatically when a change is made to the data on the associated table. A trigger can be defined to be invoked either before or after the data is changed by INSERT, UPDATE or DELETE statement. Before MySQL version 5.7.2, you can to define maximum six triggers for each table.


* **BEFORE INSERT** – activated before data is inserted into the table.
* **AFTER INSERT** – activated after data is inserted into the table.
* **BEFORE UPDATE** – activated before data in the table is updated.
* **AFTER UPDATE** – activated after data in the table is updated.
* **BEFORE DELETE** – activated before data is removed from the table.
* **AFTER DELETE** – activated after data is removed from the table.


However, from MySQL version 5.7.2+, you can define multiple triggers for the same trigger event and action time.

When you use a statement that does not use INSERT, DELETE or UPDATE statement to change data in a table, the triggers associated with the table are not invoked. For example, the TRUNCATE statement removes all data of a table but does not invoke the trigger associated with that table.

There are some statements that use the INSERT statement behind the scenes such as REPLACE statement or LOAD DATA statement. If you use these statements, the corresponding triggers associated with the table are invoked.

You must use a unique name for each trigger associated with a table. However, you can have the same trigger name defined for different tables though it is a good practice.

You should name the triggers using the following naming convention:

```
(BEFORE | AFTER)_tableName_(INSERT| UPDATE | DELETE)
```

For example, before_order_update is a trigger invoked before a row in the order table is updated.

The following naming convention is as good as the one above.

```
tablename_(BEFORE | AFTER)_(INSERT| UPDATE | DELETE)
```

For example, **order_before_update ** is the same as before_update_update trigger above.

### MySQL trigger limitations

MySQL triggers cover all features defined in the standard SQL. However, there are some limitations that you should know before using them in your applications.

MySQL triggers cannot:

* Use SHOW, LOAD DATA, LOAD TABLE, BACKUP DATABASE, RESTORE, FLUSH and RETURN statements.
* Use statements that commit or rollback implicitly or explicitly such as COMMIT , ROLLBACK , START TRANSACTION , LOCK/UNLOCK TABLES , ALTER , CREATE , DROP ,  RENAME , etc.
* Use prepared statements such as PREPARE, EXECUTE, etc.
* Use dynamic SQL statements.

From MySQL version 5.1.4, a trigger can call a stored procedure or stored function, which was a limitation is the previous versions.

### MySQL trigger syntax

In order to create a new trigger, you use the CREATE TRIGGER statement. The following illustrates the syntax of the CREATE TRIGGER statement:

```sql
CREATE TRIGGER trigger_name trigger_time trigger_event
	ON table_name
	FOR EACH ROW
 BEGIN
 ...
 END;
```
Let’s examine the syntax above in more detail.

You put the trigger name after the CREATE TRIGGER statement. The trigger name should follow the naming convention [trigger time]_[table name]_[trigger event], for example before_employees_update.

Trigger activation time can be BEFORE or AFTER. You must specify the activation time when you define a trigger. You use the BEFORE keyword if you want to process action prior to the change is made on the table and AFTER if you need to process action after the change is made.

The trigger event can be INSERT, UPDATE or DELETE. This event causes the trigger to be invoked. A trigger only can be invoked by one event. To define a trigger that is invoked by multiple events, you have to define multiple triggers, one for each event.
A trigger must be associated with a specific table. Without a table trigger would not exist therefore you have to specify the table name after the ON keyword.
You place the SQL statements between BEGIN and END block. This is where you define the logic for the trigger.

### MySQL trigger example

Let’s start creating a trigger in MySQL to log the changes of the employees table.

```sql
CREATE TABLE `employees` (
  `employeeNumber` int(11) NOT NULL,
  `lastName` varchar(50) NOT NULL,
  `firstName` varchar(50) NOT NULL,
  `extension` varchar(10) NOT NULL,
  `email` varchar(100) NOT NULL,
  `officeCode` varchar(10) NOT NULL,
  `reportsTo` int(11) DEFAULT NULL,
  `jobTitle` varchar(50) NOT NULL,
  PRIMARY KEY (`employeeNumber`)
);
```
```sql
insert  into `employees`(`employeeNumber`,`lastName`,`firstName`,`extension`,`email`,`officeCode`,`reportsTo`,`jobTitle`) values 

(1002,'Murphy','Diane','x5800','dmurphy@classicmodelcars.com','1',NULL,'President'),

(1056,'Patterson','Mary','x4611','mpatterso@classicmodelcars.com','1',1002,'VP Sales'),

(1076,'Firrelli','Jeff','x9273','jfirrelli@classicmodelcars.com','1',1002,'VP Marketing');

```

First, create a new table named **employees_audit** to keep the changes of the employee table. The following statement creates the employee_audit table.

```sql
CREATE TABLE employees_audit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employeeNumber INT NOT NULL,
    lastname VARCHAR(50) NOT NULL,
    changedat DATETIME DEFAULT NULL,
    action VARCHAR(50) DEFAULT NULL
);
```

Next, create a BEFORE UPDATE trigger that is invoked before a change is made to the employees table.

```sql
DELIMITER $$
CREATE TRIGGER before_employee_update 
    BEFORE UPDATE ON employees
    FOR EACH ROW 
BEGIN
    INSERT INTO employees_audit (action, employeeNumber, lastname, changedat)
                         VALUES ('update', OLD.employeeNumber, OLD.lastname, NOW())
END$$
DELIMITER ;
```

Inside the body of the trigger, we used the OLD keyword to access employeeNumber and lastname column of the row affected by the trigger.

Notice that in a trigger defined for INSERT, you can use NEW keyword only. You cannot use the OLD keyword. However, in the trigger defined for DELETE, there is no new row so you can use the OLD keyword only. In the UPDATE trigger, OLD refers to the row before it is updated and NEW refers to the row after it is updated.

After that, update the employees table to check whether the trigger is invoked.

```sql
UPDATE employees 
SET 
    lastName = 'Phan'
WHERE
    employeeNumber = 1056;
```

Finally, to check if the trigger was invoked by the UPDATE statement, you can query the employees_audit table using the following query:

```sql
SELECT 
    *
FROM
    employees_audit;
```

it should show data that reflects the trigger was actually invoked.