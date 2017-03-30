## Subqueries in WHERE

### IN Operator

The IN operator allows you to specify multiple values in a WHERE clause. The IN operator is a shorthand for multiple OR conditions.

Example 1:

```sql
-- Find customers who paid between $3 and $4
SELECT first_name,last_name 
  FROM customer,payment 
 WHERE customer.customer_id = payment.customer_id 
   AND payment.amount BETWEEN 3 AND 4; 
```

```sql
SELECT first_name,last_name 
  FROM customer 
 WHERE customer_id IN (SELECT customer_id 
                         FROM payment 
                        WHERE amount BETWEEN 3 AND 4); 
```

Just adding DISTINCT to the first query gives us the same result.

Example 2:

```sql
SELECT first_name 
  FROM customer,payment 
 WHERE customer.customer_id = payment.customer_id 
   AND payment.amount = 0.99 
   AND first_name LIKE ( 'W%' ) 
 ORDER BY first_name; 
```

```sql
SELECT first_name 
  FROM customer 
 WHERE customer_id IN (SELECT customer_id 
                         FROM payment 
                        WHERE amount = 0.99) 
   AND first_name LIKE ( 'W%' ) 
 ORDER BY first_name; 
```

In this case, adding DISTINCT gives us the wrong result. So we have to use a subquery.

### EXCEPT as subquery

```sql
SELECT first_name, last_name 
  FROM customer 
 WHERE customer_id IN (SELECT customer_id 
                         FROM payment 
                        WHERE amount = 0.99) 
   AND customer_id NOT IN (SELECT customer_id 
                             FROM payment 
                            WHERE amount = 1.99) 
   AND first_name LIKE ( 'W%' ) 
```

### EXISTS Operator

The EXISTS operator is used to test for the existence of any record in a subquery. The EXISTS operator returns true if the subquery returns one or more records.

Example: find customers that share the first name.

If you run this query it will give you the wrong result.

```sql
SELECT first_name,last_name 
  FROM customer c1 
 WHERE EXISTS (SELECT * 
                 FROM customer c2 
                WHERE c1.first_name = c2.first_name) 
```

This one gives you the correct one instead (a customer should not be compared against itself):

```sql
SELECT first_name,last_name 
  FROM customer c1 
 WHERE EXISTS (SELECT * 
                 FROM customer c2 
                WHERE c1.first_name = c2.first_name 
                  AND c1.customer_id <> c2.customer_id) 
```

#### Finding max

```sql
-- Find films with the max duration
SELECT title,`length` 
  FROM film f1 
 WHERE NOT EXISTS (SELECT * 
                     FROM film f2 
                    WHERE f2.`length` > f1.`length`); 
```

