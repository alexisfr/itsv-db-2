## Exercises
 1. List all the actors that share the last name. Show them in order
 2. Find actors that don't work in any film
 3. Find customers that rented only one film
 4. Find customers that rented more than one film
 5. List the actors that acted in 'BETRAYED REAR' or in 'CATCH AMISTAD'
 6. List the actors that acted in 'BETRAYED REAR' but not in 'CATCH AMISTAD'
 7. List the actors that acted in both 'BETRAYED REAR' and 'CATCH AMISTAD'
 8. List all the actors that didn't work in 'BETRAYED REAR' or 'CATCH AMISTAD'	


## Results

```sql 
-- 1. List all the actors that share the last name. Show them in order
SELECT first_name,last_name 
  FROM actor a1 
 WHERE EXISTS (SELECT * 
                 FROM actor a2 
                WHERE a1.last_name = a2.last_name
                  AND a1.actor_id <> a2.actor_id)
ORDER BY last_name

--  2. Find actors that don't work in any film
SELECT first_name, last_name
  FROM actor a1
 WHERE actor_id NOT IN (SELECT DISTINCT actor_id
 						  FROM film_actor)

-- 3. Find customers that rented only one film

/* Check how many rents per customer are in the DB
SELECT customer_id, count(1) 
  FROM rental
GROUP BY 1 
ORDER BY 2;
*/

-- Run something like this to leave one entry in rental
-- SELECT rental_id FROM rental WHERE customer_id = 281;
-- DELETE FROM rental WHERE rental_id <> 650 AND customer_id = 281
					  
SELECT c.customer_id, first_name, last_name
  FROM rental r1, customer c
 WHERE c.customer_id = r1.customer_id
   AND NOT EXISTS (SELECT *
                     FROM rental r2
 		            WHERE r1.customer_id = r2.customer_id
 		              AND r1.rental_id <> r2.rental_id )

-- 4. Find customers that rented more than one film
SELECT DISTINCT c.customer_id, first_name, last_name
  FROM rental r1, customer c
 WHERE c.customer_id = r1.customer_id
   AND EXISTS (SELECT *
                 FROM rental r2
	            WHERE r1.customer_id = r2.customer_id
	              AND r1.rental_id <> r2.rental_id )
	              
-- 5. List the actors that acted in 'BETRAYED REAR' or in 'CATCH AMISTAD'
SELECT DISTINCT actor.actor_id, actor.first_name, actor.last_name
  FROM film_actor, actor, film
 WHERE actor.actor_id = film_actor.actor_id
   AND film.film_id = film_actor.film_id
   AND title IN ('BETRAYED REAR', 'CATCH AMISTAD')

-- OR				       
SELECT actor_id, first_name, last_name  	
  FROM actor
 WHERE actor_id IN (SELECT actor.actor_id
					  FROM film_actor, actor, film
				     WHERE actor.actor_id = film_actor.actor_id
				       AND film.film_id = film_actor.film_id
				       AND title IN ('BETRAYED REAR', 'CATCH AMISTAD'))
 		          
-- 6. List the actors that acted in 'BETRAYED REAR' 
-- but not in 'CATCH AMISTAD'
SELECT actor_id, first_name, last_name  	
  FROM actor
 WHERE actor_id IN (SELECT actor.actor_id
					  FROM film_actor, actor, film
				     WHERE actor.actor_id = film_actor.actor_id
				       AND film.film_id = film_actor.film_id
				       AND title = 'BETRAYED REAR')
   AND actor_id NOT IN (SELECT actor.actor_id
					  	  FROM film_actor, actor, film
				         WHERE actor.actor_id = film_actor.actor_id
				           AND film.film_id = film_actor.film_id
				           AND title = 'CATCH AMISTAD')
				       
-- 7. List the actors that acted in both 'BETRAYED REAR' and 'CATCH AMISTAD'
SELECT actor_id, first_name, last_name  	
  FROM actor
 WHERE actor_id IN (SELECT actor.actor_id
					  FROM film_actor, actor, film
				     WHERE actor.actor_id = film_actor.actor_id
				       AND film.film_id = film_actor.film_id
				       AND title = 'BETRAYED REAR')
   AND actor_id IN (SELECT actor.actor_id
					  	  FROM film_actor, actor, film
				         WHERE actor.actor_id = film_actor.actor_id
				           AND film.film_id = film_actor.film_id
				           AND title = 'CATCH AMISTAD')

-- 8. List all the actors that didn't work in 'BETRAYED REAR' or 'CATCH AMISTAD'	
/* Wrong answer
SELECT DISTINCT actor.actor_id, first_name, last_name
  FROM film_actor, actor, film
 WHERE actor.actor_id = film_actor.actor_id
   AND film.film_id = film_actor.film_id
   AND title NOT IN ('BETRAYED REAR', 'CATCH AMISTAD');
*/
-- Right answer
SELECT actor_id, first_name, last_name  	
  FROM actor a
 WHERE NOT EXISTS (SELECT *
				     FROM film_actor, actor, film
			        WHERE actor.actor_id = film_actor.actor_id
			          AND film.film_id = film_actor.film_id
			          AND title IN ('BETRAYED REAR', 'CATCH AMISTAD')
			          AND actor.actor_id = a.actor_id);	

-- Also this way:

SELECT actor_id, first_name, last_name  	
  FROM actor a
 WHERE actor_id NOT IN (SELECT actor.actor_id
				     FROM film_actor, actor, film
			        WHERE actor.actor_id = film_actor.actor_id
			          AND film.film_id = film_actor.film_id
			          AND title IN ('BETRAYED REAR', 'CATCH AMISTAD'))	       
```
