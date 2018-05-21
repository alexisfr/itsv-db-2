## Exercises
1. Get the amount of cities per country in the database. Sort them by country, country_id.

2. Get the amount of cities per country in the database. Show only the countries with more than 10 cities, order from the highest amount of cities to the lowest   

3. Generate a report with customer (first, last) name, address, total films rented and the total money spent renting films. 
   - Show the ones who spent more money first .

4. Find all the film titles that are not in the inventory. 

5. Find all the films that are in the inventory but were never rented. 
   - Show title and inventory_id.  
   - This exercise is complicated. 
   - hint: use sub-queries in FROM and in WHERE or use left join and ask if one of the fields is null

6. Generate a report with:
    - customer (first, last) name, store id, film title, 
    - when the film was rented and returned for each of these customers
    - order by store_id, customer last_name

7. Show sales per store (money of rented films)
     - show store's city, country, manager info and total sales (money)
     - (optional) Use concat to show city and country and manager first and last name

8. Show sales per film rating

9. Which actor has appeared in the most films?

10. Which film categories have the larger film duration (comparing average)?
   - Order by average in descending order
 
## Results

-- 1
SELECT COUNT(city_id), country.country, country.country_id
  FROM country 
  	INNER JOIN city USING (country_id)
GROUP BY country.country,country.country_id
ORDER BY country.country,country.country_id;

-- 2

SELECT COUNT(city_id), country.country
  FROM country 
  	INNER JOIN city USING (country_id)
GROUP BY country.country
	HAVING COUNT(*) > 10
ORDER BY 1 DESC;

-- 3

SELECT customer.first_name, customer.last_name, address.address, COUNT(rental.rental_id), SUM(payment.amount)
  FROM customer 
  	INNER JOIN address USING (address_id)
  	INNER JOIN rental USING (customer_id)
  	INNER JOIN payment USING(rental_id)
GROUP BY 1, 2, 3
ORDER BY 5 DESC;

-- 4

SELECT film.title
  FROM film
WHERE film.film_id NOT IN 
	(SELECT film_id FROM inventory);
	
-- 5

SELECT film.title, inventory_id, rental.rental_id
  FROM film 
  	INNER JOIN inventory USING (film_id)
  	LEFT OUTER JOIN rental USING (inventory_id)
WHERE rental.rental_id IS NULL;

-- 6

SELECT customer.first_name, customer.last_name, inventory.store_id, film.title, rental.rental_date, rental.return_date
  FROM film
  	INNER JOIN inventory USING (film_id)
  	INNER JOIN rental USING (inventory_id)
  	INNER JOIN customer USING (customer_id)
WHERE rental.return_date IS NOT NULL
ORDER BY inventory.store_id, customer.last_name;

-- 7

SELECT
CONCAT(c.city, _utf8',', cy.country) AS store
, CONCAT(m.first_name, _utf8' ', m.last_name) AS manager
, SUM(p.amount) AS total_sales
FROM payment AS p
INNER JOIN rental AS r ON p.rental_id = r.rental_id
INNER JOIN inventory AS i ON r.inventory_id = i.inventory_id
INNER JOIN store AS s ON i.store_id = s.store_id
INNER JOIN address AS a ON s.address_id = a.address_id
INNER JOIN city AS c ON a.city_id = c.city_id
INNER JOIN country AS cy ON c.country_id = cy.country_id
INNER JOIN staff AS m ON s.manager_staff_id = m.staff_id
GROUP BY s.store_id
ORDER BY cy.country, c.city;

-- 8
SELECT rating, sum(amount)
from film
inner join inventory using (film_id)
inner join rental using(inventory_id)
inner join payment using(rental_id)
group by 1

-- 9
select actor.actor_id, actor.first_name, actor.last_name,
       count(actor_id) as film_count
from actor join film_actor using (actor_id)
group by actor_id
order by film_count desc
limit 1;

-- 10
select category.name, avg(length)
from film join film_category using (film_id) join category using (category_id)
group by category.name
having avg(length) > (select avg(length) from film)
order by avg(length) desc;