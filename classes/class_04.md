## Set Operators

- UNION
- INTERSECT (Not supported by Mysql)
- EXCEPT (Not supported by Mysql)

### UNION
```sql
SELECT name AS val FROM category
WHERE name LIKE 'A%' OR name LIKE 'M%'
UNION 
SELECT title  FROM film
WHERE title LIKE 'A%' OR title LIKE 'S%'
```
Union eliminates duplicates and sort the result, to see all the values UNION ALL has to be used.

## Table Variables

```sql
SELECT f.title, f.special_features, f.rental_rate, c.name
 FROM film f, film_category fc,  category c 
WHERE f.film_id = fc.film_id 
  AND fc.category_id = c.category_id
ORDER BY f.rental_rate DESC, f.special_features ASC
```

```sql
SELECT f1.title, f2.title, f1.`length` 
  FROM film f1, film f2
WHERE f1.`length` = f2.`length`
```

```sql
SELECT f1.title, f2.title, f1.`length` 
  FROM film f1, film f2
WHERE f1.`length` = f2.`length` AND f1.film_id <> f2.film_id;
```

## Exercises

1. Show title and special_features of films that are PG-13
2. Get a list of all the different films duration.
3. Show title, rental_rate and replacement_cost of films that have replacement_cost from 20.00 up to 24.00
4. Show title, category and rating of films that have 'Behind the Scenes'   as special_features 
5. Show first name and last name of actors that acted in 'ZOOLANDER FICTION'
6. Show the address, city and country of the store with id 1
7. Show pair of film titles and rating of films that have the same rating.
8. Get all the films that are available in store id 2 and the manager first/last name of this store (the manager will appear in all the rows).
