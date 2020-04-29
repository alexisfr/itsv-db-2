## ALL / ANY

**ALL** means “return TRUE if the comparison is TRUE for ALL of the values in the column that the subquery returns.” 
For example:

```sql
SELECT s1 FROM t1 WHERE s1 > ALL (SELECT s1 FROM t2);
```

Suppose that there is a row in table t1 containing (10). The expression is TRUE if table t2 contains (-5,0,+5)
because 10 is greater than all three values in t2.
The expression is FALSE if table t2 contains (12,6,NULL,-100) because there is a single value 12 in table t2 
that is greater than 10. The expression is unknown (that is, NULL) if table t2 contains (0,NULL,1).

Finally, the expression is TRUE if table t2 is empty. So, the following expression is TRUE when table t2 is empty:
```sql
SELECT * FROM t1 WHERE 1 > ALL (SELECT s1 FROM t2);
```

```sql
SELECT title,length 
  FROM film 
 WHERE length >= ALL (SELECT length 
                        FROM film);
```                         


```sql
UPDATE film SET length = 200 WHERE film_id = 182;

SELECT title,length 
  FROM film f1 
 WHERE length > ALL (SELECT length 
                       FROM film f2
                      WHERE f2.film_id <> f1.film_id);
```


**ANY** means  “return TRUE if the comparison is TRUE for ANY of the values in the column that the subquery returns.”
For example:

```sql
SELECT s1 FROM t1 WHERE s1 > ANY (SELECT s1 FROM t2);
```

Suppose that there is a row in table t1 containing (10). The expression is TRUE if table t2 contains (21,14,7) 
because there is a value 7 in t2 that is less than 10. 
The expression is FALSE if table t2 contains (20,10), or if table t2 is empty

```sql
SELECT title,length 
  FROM film f1 
 WHERE NOT length <= ANY (SELECT length 
                       FROM film f2 
                      WHERE f2.film_id <> f1.film_id);

UPDATE film SET length = 185 WHERE film_id = 182;                       
```

```sql
-- Films whose replacement cost is higher than the lowest replacement cost 
SELECT title,replacement_cost 
  FROM film 
 WHERE replacement_cost > ANY (SELECT replacement_cost 
                                 FROM film) 
 ORDER BY replacement_cost; 

-- Same query with exists
 SELECT title,replacement_cost 
  FROM film f1 
 WHERE EXISTS (SELECT * 
                 FROM film f2 
                WHERE f1.replacement_cost > f2.replacement_cost) 
 ORDER BY replacement_cost; 
```

## Subqueries in FROM

```sql
SELECT title,description,
rental_rate,
rental_rate * 15 AS in_pesos 
  FROM film 
 WHERE rental_rate * 15 > 10.0 
   AND rental_rate * 15 < 70.0;

-- Can be written

SELECT * 
  FROM (SELECT title,description,rental_rate,rental_rate * 15 AS in_pesos 
          FROM film) g 
 WHERE in_pesos > 10.0 
   AND in_pesos < 70.0; 

```    

## Subqueries in SELECT

This is an example to show that this can also be done, there are simpler ways of
doing this:

```sql
SELECT customer_id, 
       first_name, 
       last_name, 
       (SELECT DISTINCT amount 
          FROM payment 
         WHERE customer.customer_id = payment.customer_id 
           AND amount >= ALL (SELECT amount 
                                FROM payment 
                               WHERE customer.customer_id = payment.customer_id))
	   AS max_amount 
  FROM customer 
 ORDER BY max_amount DESC, 
        customer_id DESC;
``` 
Is equivalent to

```sql
SELECT customer_id,
	   first_name,
	   last_name,
	   (SELECT MAX(amount) 
	      FROM payment 
	     WHERE payment.customer_id = customer.customer_id) AS amount
  FROM customer
 ORDER BY amount DESC,
          customer_id DESC;
``` 

```sql
SELECT customer.customer_id, 
       first_name, 
       last_name, 
       MAX(amount) max_amount 
  FROM customer, payment 
 WHERE customer.customer_id = payment.customer_id 
 GROUP BY customer_id, first_name, last_name 
 ORDER BY max_amount DESC, customer_id DESC 
 ```

## Exercises

1. Find the films with less duration, show the title and rating.
2. Write a query that returns the tiltle of the film which duration is the lowest. If there are more than one film with the lowest durtation, the query returns an empty resultset.
3. Generate a report with list of customers showing the lowest payments done by each of them. Show customer information, the address and the lowest amount, provide both solution using ALL and/or ANY and MIN.
4. Generate a report that shows the customer's information with the highest payment and the lowest payment in the same row.
