# Class 13:

1- Add a new customer
    - To store 1
    - For address use an existing address. The one that has the biggest address_id in 'United States' 

```sql
INSERT INTO sakila.customer
(store_id, first_name, last_name, email, address_id, active)
SELECT 1, 'Pepe', 'Suarez', 'pepesuarez@gmail.com', MAX(a.address_id), 1
FROM address a
WHERE (SELECT c.country_id
		FROM country c, city c1
		WHERE c.country = "United States"
		AND c.country_id = c1.country_id
		AND c1.city_id = a.city_id);
		

SELECT *
FROM customer
WHERE last_name = "Suarez";
```

2- Add a rental
   - Make easy to select any film title. I.e. I should be able to put 'film tile' in the where, and not the id.
   - Do not check if the film is already rented, just use any from the inventory, e.g. the one with highest id.
   - Select any staff_id from Store 2. 

```sql
INSERT INTO sakila.rental
(rental_date, inventory_id, customer_id, return_date, staff_id)
SELECT CURRENT_TIMESTAMP, 
		(SELECT MAX(r.inventory_id)
		 FROM inventory r
		 INNER JOIN film USING(film_id)
		 WHERE film.title = "ARABIA DOGMA" -- Put film here
		 LIMIT 1), 
		 600, -- Put user here (in this case is the previous one insterted in excersise 1
		 NULL,
		 (SELECT staff_id
		  FROM staff
		  INNER JOIN store USING(store_id)
		  WHERE store.store_id = 2
		  LIMIT 1);
```

3- Update film year based on the rating
   - For example if rating is 'G' release date will be '2001'
   - You can choose the mapping between rating and year.
   - Write as many statements are needed.

```sql
UPDATE sakila.film
SET release_year='2001'
WHERE rating = "G";

UPDATE sakila.film
SET release_year='2005'
WHERE rating = "PG";

UPDATE sakila.film
SET release_year='2010'
WHERE rating = "PG-13";

UPDATE sakila.film
SET release_year='2015'
WHERE rating = "R";

UPDATE sakila.film
SET release_year='2020'
WHERE rating = "NC-17";
```

4- Return a film
   - Write the necessary statements and queries for the following steps.
   - Find a film that was not yet returned. And use that rental id. Pick the latest that was rented for example.
   - Use the id to return the film.

```sql
#Rental id devuelto 11496 , el precio 2,99, el customer = 155, staff = 1
SELECT rental_id, rental_rate, customer_id, staff_id
FROM film
INNER JOIN inventory USING(film_id)
INNER JOIN rental USING(inventory_id)
WHERE rental.return_date IS NULL
LIMIT 1;

#Hago un update a rental para decir que la pelicula fue devuelta
UPDATE sakila.rental
SET  return_date=CURRENT_TIMESTAMP
WHERE rental_id=11496;
```
   
5- Try to delete a film
   - Check what happens, describe what to do.
   - Write all the necessary delete statements to entirely remove the film from the DB.

```sql
DELETE FROM payment
 WHERE rental_id IN (SELECT rental_id 
                       FROM rental
                      INNER JOIN inventory USING (inventory_id) 
                      WHERE film_id = 1);
 
DELETE FROM rental
 WHERE inventory_id IN (SELECT inventory_id 
                         FROM inventory
                        WHERE film_id = 1);
                        
DELETE FROM inventory WHERE film_id = 1;

DELETE film_actor FROM film_actor WHERE film_id = 1;

DELETE film_category FROM film_category WHERE film_id = 1;

DELETE film FROM film WHERE film_id = 1;

```

6- Rent a film
   - Find an inventory id that is available for rent (available in store) pick any movie. Save this id somewhere.
   - Add a rental entry
   - Add a payment entry
   - Use sub-queries for everything, except for the inventory id that can be used directly in the queries.

```sql
SELECT inventory_id, film_id

FROM inventory

WHERE inventory_id NOT IN (SELECT inventory_id

FROM inventory

	INNER JOIN rental USING (inventory_id)

	WHERE return_date IS NULL)

# inventory id to use: 10

# film id to use: 2

INSERT INTO sakila.rental

(rental_date, inventory_id, customer_id, staff_id)

VALUES(

CURRENT_DATE(),

10,

(SELECT customer_id FROM customer ORDER BY customer_id DESC LIMIT 1),

(SELECT staff_id FROM staff WHERE store_id = (SELECT store_id FROM inventory WHERE inventory_id = 10))

);

INSERT INTO sakila.payment

(customer_id, staff_id, rental_id, amount, payment_date)

VALUES(

(SELECT customer_id FROM customer ORDER BY customer_id DESC LIMIT 1),

(SELECT staff_id FROM staff LIMIT 1),

(SELECT rental_id FROM rental ORDER BY rental_id DESC LIMIT 1) ,

(SELECT rental_rate FROM film WHERE film_id = 2),

CURRENT_DATE());

```

# Class 14

```sql
-- 1

SELECT CONCAT_WS(" ",first_name,last_name) as full_name, address.address, city.city
FROM customer 
	INNER JOIN address USING(address_id)
	INNER JOIN city USING(city_id)
	INNER JOIN country USING(country_id)
WHERE country.country LIKE 'Argentina';
```

```sql
-- 2

SELECT title,
`language`.name, 
CASE
	WHEN rating = 'G' THEN 'All Ages Are Admitted.'
	WHEN rating = 'PG' THEN 'Some Material May Not Be Suitable For Children.'
	WHEN rating = 'PG-13' THEN 'Some Material May Be Inappropriate For Children Under 13.'
	WHEN rating = 'R' THEN 'Under 17 Requires Accompanying Parent Or Adult Guardian.'
	WHEN rating = 'NC-17' THEN 'No One 17 and Under Admitted.'
END AS rating_description
  FROM film
	INNER JOIN `language` USING (language_id);
```
	
```sql
-- 3
	
SELECT title, release_year
  FROM film 
	INNER JOIN film_actor USING(film_id)
	INNER JOIN actor USING(actor_id)
WHERE CONCAT_WS(" ",first_name,last_name) LIKE TRIM(UPPER("   johNNy lollobRigidA     "));
```

```sql
-- 4

SELECT film.title,
	   CONCAT_WS(" ", customer.first_name, customer.last_name) as full_name,
	   CASE WHEN rental.return_date IS NOT NULL THEN 'Yes'
	   ELSE 'No' END AS was_returned,
	   MONTHNAME(rental.rental_date) as month
  FROM film
  	INNER JOIN inventory USING(film_id)
  	INNER JOIN rental USING(inventory_id)
  	INNER JOIN customer USING(customer_id)
WHERE MONTHNAME(rental.rental_date) LIKE 'May'
   OR MONTHNAME(rental.rental_date) LIKE 'June';
```

```sql
-- 5

-- CAST and CONVERT have barely no differences between them.
-- While CAST has a slightly distinct syntax than CONVERT,they're both used to convert data from one type to another.

SELECT CAST(last_update AS DATE) as only_date
FROM rental;

SELECT CONVERT("2006-02-15", DATETIME);
```

```sql
-- 6

-- NVL() and IFNULL() functions work in the same way: 
-- they check whether an expression is NULL or not; if it is, they return a second expression (a default value).

-- NVL() is an Oracle function, so here is an IFNULL() example:

SELECT rental_id, IFNULL(return_date, 'La pelicula no fue devuelta aun') as fecha_de_devolucion
  FROM rental
WHERE rental_id = 12610
  OR rental_id = 12611;
  
-- ISNULL() function returns 1 if the expression passed is NULL, otherwise it returns 0.
  
SELECT rental_id, ISNULL(return_date) as pelicula_faltante
  FROM rental
WHERE rental_id = 12610
  OR rental_id = 12611;
  
-- COALESCE() function returns the first non-NULL argument of the passed list.
  
SELECT COALESCE(NULL,
				NULL,
				(SELECT return_date
				FROM rental
				WHERE rental_id = 12610), -- null date
				(SELECT return_date
				FROM rental
				WHERE rental_id = 12611)) as primer_valor_no_nulo;
```

# Class 15

```sql    
-- 1

CREATE OR REPLACE VIEW list_of_customers AS
	SELECT customer_id,
		CONCAT_WS(" " ,first_name, last_name) as full_name,
		address,
		postal_code,
		phone,
		city,
		country,
		CASE 
			WHEN active = 1 THEN 'active' 
			ELSE 'inactive' 
		END AS status,
		store_id
	FROM customer
		INNER JOIN address USING(address_id)
		INNER JOIN city USING(city_id)
		INNER JOIN country USING(country_id);
		
SELECT * FROM list_of_customers;
```

```sql
-- 2

CREATE OR REPLACE VIEW film_details AS
	SELECT film_id,
		title,
		description,
		name,
		rental_rate,
		`length`,
		rating,
		GROUP_CONCAT(DISTINCT CONCAT_WS(" " ,first_name, last_name) SEPARATOR ',') AS actores
	FROM film
		INNER JOIN film_category USING(film_id)
		INNER JOIN category USING(category_id)
		INNER JOIN film_actor USING(film_id)
		INNER JOIN actor USING(actor_id)
	GROUP BY 1,4;
	
SELECT * FROM film_details;
```

```sql
-- 3

CREATE OR REPLACE VIEW sales_by_film_category AS
	SELECT DISTINCT category.name, SUM(amount) as total_rental FROM category
					INNER JOIN film_category USING(category_id)
					INNER JOIN film USING(film_id)
					INNER JOIN inventory USING(film_id)
					INNER JOIN rental USING(inventory_id)
					INNER JOIN payment USING(rental_id)
				GROUP BY 1;
	
	
SELECT * FROM sales_by_film_category;
```

```sql
-- 4

CREATE OR REPLACE VIEW actor_information AS
	SELECT actor_id as id,
		first_name,
		last_name,
		(SELECT COUNT(film_id)
		FROM film
			INNER JOIN film_actor USING(film_id)
			INNER JOIN actor USING(actor_id)
		WHERE actor_id = id) AS films_participated_in
	  FROM actor;
	  
SELECT * FROM actor_information;
```

```sql
-- 5

-- This view fetches values from table actor, and joins them whenever it can with values from tables film_actor, film_category
-- and category. This meaning that they will only be joined in case the actor has acted in a film.
-- It displays the data in four columns. The first three of them are just basic actor data: its name, surname and id.
-- The fourth column, called film_info, displays every film the actor has acted on, grouped by category.
-- The query achieves this by concatenating every category with the group of films that belongs to it and, after that,
-- concatenating the groups of categories with its films together.  
```

```sql
-- 6

-- Materialized views are a form of chaching query results.The main difference between this type of views the ordinary ones  
-- is that Materialized views are concrete tables that store the results of a query.
-- They are used to improve performance and exist in a variety of DBMSs, but not in MySQL. In this last scenario you could
-- implement some workaround by using triggers or stored procedures.
```

# Class 16

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

INSERT INTO `employees`(`employeeNumber`,`lastName`,`firstName`,`extension`,`email`,`officeCode`,`reportsTo`,`jobTitle`) VALUES 
(1002,'Murphy','Diane','x5800','dmurphy@classicmodelcars.com','1',NULL,'President'),
(1056,'Patterson','Mary','x4611','mpatterso@classicmodelcars.com','1',1002,'VP Sales'),
(1076,'Firrelli','Jeff','x9273','jfirrelli@classicmodelcars.com','1',1002,'VP Marketing');

CREATE TABLE employees_audit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employeeNumber INT NOT NULL,
    lastname VARCHAR(50) NOT NULL,
    changedat DATETIME DEFAULT NULL,
    action VARCHAR(50) DEFAULT NULL
);
```

```sql
-- 1

-- It throws an error because of the NOT NULL constraint we added on the email attribute when we created the table.
    
INSERT INTO `employees`(`employeeNumber`,`lastName`,`firstName`,`extension`,`email`,`officeCode`,`reportsTo`,`jobTitle`) VALUES 
(1056,'Patterson','Mary','x4611',NULL,'1',1002,'VP Sales');
```

```sql
-- 2

UPDATE employees set employeeNumber = employeeNumber - 20;

-- Esa querie funciona y actualiza todas las rows, porque el motor la ejecuta sequencialmente el update, por lo que aunque la distancia entre 2 ids es 20 el update ocurre primero en la fila del medio y por lo tanto nunca coliciona.

UPDATE employees set employeeNumber = employeeNumber + 20;

-- En este caso, cuando la fila del medio actualiza su valor, este es el mismo que la de la tercer fila por lo tanto tanto no puede hacer el update por el constraint de la primary key.

```

```sql
-- 3

ALTER TABLE employees
ADD age TINYINT UNSIGNED DEFAULT 69;

ALTER TABLE employees
   ADD CONSTRAINT age CHECK(age >= 16 AND age <= 70);
```

```sql
-- 5
   
ALTER TABLE employees
	ADD COLUMN lastUpdate DATETIME;

	
ALTER TABLE employees
	ADD COLUMN lastUpdateUser VARCHAR(255);	


CREATE TRIGGER before_employee_update 
    BEFORE UPDATE ON employees
    FOR EACH ROW 
BEGIN
     SET NEW.lastUpdate=NOW();
     SET NEW.lastUpdateUser=CURRENT_USER;
END;


update employees set lastName = 'Phanny' where employeeNumber = 1076;
```

```sql
-- 6

-- ins_film Inserts a new film_text entry, with the same values as the added film.

BEGIN
    INSERT INTO film_text (film_id, title, description)
        VALUES (new.film_id, new.title, new.description);
END

-- upd_film Updates the corresponding existing film_text entry for the updated film. 

BEGIN
	IF (old.title != new.title) OR (old.description != new.description) OR (old.film_id != new.film_id)
	THEN
	    UPDATE film_text
	        SET title=new.title,
	            description=new.description,
	            film_id=new.film_id
	    WHERE film_id=old.film_id;
	END IF;
END

-- del_film Deletes the corresponding existing film_text entry for the deleted film. 

BEGIN
    DELETE FROM film_text WHERE film_id = old.film_id;
END
```