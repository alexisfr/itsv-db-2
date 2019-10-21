## Introduction to user accounts in MySQL
MySQL Create UserIn MySQL, you can specify not only who can connect to the database server but also from which host that the user connects. Therefore, a user account in MySQL consists of a username and a host name separated by the @ character.


For example, if the admin user connects to the MySQL database server from localhost, the user account is admin@localhost


The admin user only can connect to the MySQL database server from the localhost, not from a remote host such as apache.org. This makes the MySQL database server even more secure.

In addition, by combining the username and host, it is possible to setup multiple accounts with the same name but can connect from different hosts with the different privileges.

MySQL stored the user accounts in the user grant table of the mysql database.

### Creating user accounts using MySQL CREATE USER statement
MySQL provides the CREATE USER  statement that allows you to create a new user account. The syntax of the CREATE USER statement is as follows:

```sql
CREATE USER user_account IDENTIFIED BY password;
```

The user_account in the format 'username'@'hostname'  is followed by the CREATE USER clause.

The password is specified in the IDENTIFIED BY clause. The password must be in clear text. MySQL will encrypt the password before saving the user account into the user table.

For example, to create a new user dbadmin that connects to the MySQL database server from the localhost with the password secret, you use the CREATE USER  statement as follows:

```sql
CREATE USER dbadmin@localhost 
IDENTIFIED BY 'secret';
```

To view the privileges of a user account, you use the SHOW GRANTS statement as follows:

```sql
SHOW GRANTS FOR dbadmin@localhost;
```

To allow a user account to connect from any host, you use the percentage (%) wildcard as shown in the following example:

```sql
CREATE USER superadmin@'%'
IDENTIFIED BY 'secret';
```

The percentage wildcard % has the same effect as it is used in the LIKE operator e.g., to allow mysqladmin user account to connect to the database server from any subdomain.

### Change MySQL user password
####  Using UPDATE statement
The first way to change the password is to use the UPDATE statement to update the user table of the mysql database.

```sql
-- MYSQL < 5.7.6
USE mysql;
 
UPDATE user 
SET password = PASSWORD('dolphin')
WHERE user = 'dbadmin' AND 
      host = 'localhost';
 
FLUSH PRIVILEGES;
```
```sql
-- MYSQL  5.7.6+
USE mysql;
 
UPDATE user 
SET authentication_string = PASSWORD('dolphin')
WHERE user = 'dbadmin' AND 
      host = 'localhost';
 
FLUSH PRIVILEGES;
```

#### Using the SET PASSWORD statement

```sql
SET PASSWORD FOR 'dbadmin'@'localhost' = PASSWORD('bigshark');
```

### Using ALTER USER statement

```sql
ALTER USER dbadmin@localhost IDENTIFIED BY 'littlewhale';
```

## MySQL GRANT statement
After creating a new user account, the user doesn’t have any privileges. To grant privileges to a user account, you use the GRANT statement.

The following illustrates the syntax of the GRANT statement:

```sql
GRANT privilege,[privilege],.. ON privilege_level 
TO user [IDENTIFIED BY password]
[REQUIRE tsl_option]
[WITH [GRANT_OPTION | resource_option]];
```

Let’s examine the GRANT statement in greater detail.

- First, specify one or more privileges after the GRANT keyword. If you grant the user multiple privileges, each privilege is separated by a comma. (see a list of privilege in the table below).

- Next, specify the privilege_level that determines the level at which the privileges apply. MySQL supports global ( *.*), database ( database.*), table ( database.table) and column levels. If you use column privilege level, you must specify one or a list of comma-separated column after each privilege.

- Then, place the user that you want to grant privileges.  If user already exists, the GRANT statement modifies its privilege. Otherwise, the GRANT statement creates a new user. The optional clauseIDENTIFIED BY allows you set a new password for the user.

- After that, you specify whether the user has to connect to the database server over a secure connection such as SSL, X059, etc.

- Finally, the optional WITH GRANT OPTION clause allows you to grant other users or remove from other users the privileges that you possess. In addition, you can use the WITH clause to allocate MySQL database server’s resource e.g., to set how many connections or statements that the user can use per hour. This is very helpful in the shared environments such as MySQL shared hosting.

Notice that in order to use the GRANT statement, you must have the GRANT OPTION privilege and the privileges that you are granting. If the read_only system variable is enabled, you need to have the SUPER privilege to execute the GRANT statement.

### Examples
```sql
CREATE USER super@localhost IDENTIFIED BY 'dolphin';
```

```sql
SHOWS  GRANTS FOR super@localhost;
```

```sql
GRANT ALL ON *.* TO 'super'@'localhost' WITH GRANT OPTION;
```

### Grant Details


| Privilege               | Meaning                                                                                                                                 | Global | Database | Table | Column | Procedure | Proxy |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|--------|----------|-------|--------|-----------|-------|
| ALL [PRIVILEGES]        | Grant all privileges at specified access level except GRANT OPTION                                                                      |        |          |       |        |           |       |
| ALTER                   | Allow user to use of ALTER TABLE statement                                                                                              | X      | X        | X     |        |           |       |
| ALTER ROUTINE           | Allow user to alter or drop stored routine                                                                                              | X      | X        |       |        | X         |       |
| CREATE                  | Allow user to create database and table                                                                                                 | X      | X        | X     |        |           |       |
| CREATE ROUTINE          | Allow user to create stored routine                                                                                                     | X      | X        |       |        |           |       |
| CREATE TABLESPACE       | Allow user to create, alter or drop tablespaces and log file groups                                                                     | X      |          |       |        |           |       |
| CREATE TEMPORARY TABLES | Allow user to create temporary table by using CREATE TEMPORARY TABLE                                                                    | X      | X        |       |        |           |       |
| CREATE USER             | Allow user to use the CREATE USER, DROP USER, RENAME USER, and REVOKE ALL PRIVILEGES statements.                                        | X      |          |       |        |           |       |
| CREATE VIEW             | Allow user to create or modify view.                                                                                                    | X      | X        | X     |        |           |       |
| DELETE                  | Allow user to use DELETE                                                                                                                | X      | X        | X     |        |           |       |
| DROP                    | Allow user to drop database, table and view                                                                                             | X      | X        | X     |        |           |       |
| EVENT                   | Enable use of events for the Event Scheduler.                                                                                           | X      | X        |       |        |           |       |
| EXECUTE                 | Allow user to execute stored routines                                                                                                   | X      | X        | X     |        |           |       |
| FILE                    | Allow user to read any file in the database directory.                                                                                  | X      |          |       |        |           |       |
| GRANT OPTION            | Allow user to have privileges to grant or revoke privileges from other accounts.                                                        | X      | X        | X     |        | X         | X     |
| INDEX                   | Allow user to create or remove indexes.                                                                                                 | X      | X        | X     |        |           |       |
| INSERT                  | Allow user to use INSERT statement                                                                                                      | X      | X        | X     | X      |           |       |
| LOCK TABLES             | Allow user to use LOCK TABLES on tables for which you have the SELECT privilege                                                         | X      | X        |       |        |           |       |
| PROCESS                 | Allow user to see all processes with SHOW PROCESSLIST statement.                                                                        | X      |          |       |        |           |       |
| PROXY                   | Enable user proxying.                                                                                                                   |        |          |       |        |           |       |
| REFERENCES              | Allow user to create foreign key                                                                                                        | X      | X        | X     | X      |           |       |
| RELOAD                  | Allow user to use FLUSH operations                                                                                                      | X      |          |       |        |           |       |
| REPLICATION CLIENT      | Allow user to query to see where master or slave servers are                                                                            | X      |          |       |        |           |       |
| REPLICATION SLAVE       | Allow the user to use replicate slaves to read binary log events from the master.                                                       | X      |          |       |        |           |       |
| SELECT                  | Allow user to use SELECT statement                                                                                                      | X      | X        | X     | X      |           |       |
| SHOW DATABASES          | Allow user to show all databases                                                                                                        | X      |          |       |        |           |       |
| SHOW VIEW               | Allow user to use SHOW CREATE VIEW statement                                                                                            | X      | X        | X     |        |           |       |
| SHUTDOWN                | Allow user to use mysqladmin shutdown command                                                                                           | X      |          |       |        |           |       |
| SUPER                   | Allow user to use other administrative operations such as CHANGE MASTER TO, KILL, PURGE BINARY LOGS, SET GLOBAL, and mysqladmin command | X      |          |       |        |           |       |
| TRIGGER                 | Allow user to use TRIGGER operations.                                                                                                   | X      | X        | X     |        |           |       |
| UPDATE                  | Allow user to use UPDATE statement                                                                                                      | X      | X        | X     | X      |           |       |
| USAGE                   | Equivalent to “no privileges”                                                                                                           |        |          |       |        |           |       |

## Revoke

Introduction to the MySQL REVOKE Statement
In order to revoke privileges from a user account, you use the MySQL REVOKE statement. MySQL allows you to revoke one or more privileges or all privileges from a user.

The following illustrates the syntax of revoking specific privileges from a user:

```sql
REVOKE   privilege_type [(column_list)]      
        [, priv_type [(column_list)]]...
ON [object_type] privilege_level
FROM user [, user]...
```

Let’s examine the MySQL REVOKE statement in more detail.

- First, specify a list of privileges that you want to revoke from a user right after the REVOKE keyword. You need to separate privileges by commas.
- Second, specify the privilege level at which privileges is revoked in the ON clause .
- Third, specify the user account that you want to revoke the privileges in the FROM clause.

#### Examples

```sql
-- revoke DELETE from all tables in classicmodels database	
REVOKE  DELETE ON classicmodels.*  FROM myuser;
```

```sql
-- revoke all the permissions
REVOKE ALL PRIVILEGES, GRANT OPTION FROM myuser;
```
## Exercises

1. Create a user **data_analyst**
2. Grant permissions only to SELECT, UPDATE and DELETE to all sakila tables to it.
3. Login with this user and try to create a table. Show the result of that operation.
4. Try to update a title of a film. Write the update script.
5. With **root** or any admin user revoke the UPDATE permission. Write the command
6. Login again with **data_analyst** and try again the update done in step 4. Show the result.
