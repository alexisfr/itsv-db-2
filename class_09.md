## Having

The HAVING clause is used in combination with the GROUP BY clause to restrict the groups of returned rows to only those whose the condition is TRUE.

#### Syntax

```sql
SELECT expression1, expression2, ... expression_n, 
       aggregate_function (expression)
FROM tables
[WHERE conditions]
GROUP BY expression1, expression2, ... expression_n
HAVING condition;
```

#### Example

```sql
-- Find customers that rented only one film
SELECT c.customer_id, first_name, last_name, COUNT(*)
  FROM rental r1, customer c
 WHERE c.customer_id = r1.customer_id
GROUP BY c.customer_id, first_name, last_name
HAVING COUNT(*) = 1
```

```sql
-- Show the films' ratings where the minimum film duration in that group is greater than 46
SELECT rating, MIN(`length`)
FROM film
GROUP BY rating
HAVING MIN(`length`) > 46
```

```sql
-- Show ratings that have less than 195 films
SELECT rating, COUNT(*) AS total
FROM film
GROUP BY rating
HAVING COUNT(*) < 195

-- same but with subqueries
SELECT DISTINCT rating,
(SELECT COUNT(*) FROM film f3 WHERE f3.rating = f1.rating) AS total
FROM film f1
WHERE (SELECT COUNT(*) 
FROM film f2 WHERE f1.rating = f2.rating) < 195
```

```sql
-- Show ratings where their film duration average is grater than all films duration average.
SELECT rating, AVG(`length`)
FROM film
GROUP BY rating
HAVING AVG(`length`) > (SELECT AVG(`length`) FROM film)
```

### Exercises:

1. Get the amount of cities per country in the database. Sort them by country, country_id.

2. Get the amount of cities per country in the database. Show only the countries with more than 10 cities, order from the highest amount of cities to the lowest   

3. Generate a report with customer (first, last) name, address, total films rented and the total money spent renting films. 
   - Show the ones who spent more money first .

4. Which film categories have the larger film duration (comparing average)?
   - Order by average in descending order

5. Show sales per film rating