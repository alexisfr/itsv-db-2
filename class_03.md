## Installing Mysql sakila DB with Docker 
### Prerequisites
- Docker https://docs.docker.com/engine/getstarted/linux_install_help/

```bash
$ sudo docker kill mysql
$ sudo docker rm mysql
$ sudo docker run --name mysql -e MYSQL_ROOT_PASSWORD=password -d -p 3306:3306 mysql:latest

$ sudo docker exec -it mysql mysql -uroot -p

mysql> CREATE USER 'user'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL ON *.* TO 'user'@'%';
mysql> quit

$ wget http://downloads.mysql.com/docs/sakila-db.tar.gz
$ tar -zxvf sakila-db.tar.gz

$ cd sakila-db

$ sudo docker exec -i mysql mysql -uuser -ppassword < sakila-schema.sql
$ sudo docker exec -i mysql mysql -uuser -ppassword < sakila-data.sql

```
# Generate ER diagram

Review the diagram.

![sakila-tables](/uploads/c96ed6d838cfa072dc8fbe0a5effd9b9/sakila-tables.png)

# DML
Data manipulation language operations are:

- SELECT
- INSERT
- UPDATE

## SELECT

```sql
SELECT A1, A2, A3..., An
FROM T1, T2, T3,..., Tn
WHERE condition
```

### Operators in The WHERE Clause

| Comparison Operator | Description                                           |
|---------------------|-------------------------------------------------------|
| =                   | Equal                                                 |
| <=>                 | Equal (MySql, safe to compare NULL values)            |
| <>                  | Not Equal                                             |
| !=                  | Not Equal                                             |
| >                   | Greater Than                                          |
| >=                  | Greater Than or Equal                                 |
| <                   | Less Than                                             |
| <=                  | Less Than or Equal                                    |
| BETWEEN             | Within a range (inclusive)                            |
| NOT                 | Negates a condition                                   |
| IS NULL             | NULL value                                            |
| IS NOT NULL         | Non-NULL value                                        |
| LIKE                | Pattern matching with % and _                         |
| IN ( )              | Matches a value in a list                             |
| EXISTS              | Condition is met if subquery returns at least one row |

# Simple queries

## Conditions
```sql
SELECT title, rating, length
FROM film
WHERE length > 100;
```
```sql
SELECT title, `length` FROM film
WHERE `length` BETWEEN 100 AND 120;
```

## Mutilple Tables
```sql
SELECT city, district
FROM address, city
WHERE address.city_id = city.city_id;
```

## Adding distinct
```sql
SELECT [DISTINCT] country, city
FROM address, city, country
WHERE address.city_id = city.city_id
AND city.country_id = country.country_id;
```

## Conditions with columns in different tables 
```sql
SELECT title, name
FROM film, `language`
WHERE film.language_id = language.language_id
AND film.`length` > 100 AND language.name = 'English'
```

## Ambigous column names
```sql
SELECT title, category_id
 FROM film, film_category, category
WHERE film.film_id = film_category.film_id 
  AND film_category.category_id = category.category_id
``` 

## Adding Order BY
```sql
SELECT title, special_features, rental_rate, name
 FROM film, film_category, category
WHERE film.film_id = film_category.film_id 
  AND film_category.category_id = category.category_id
ORDER BY rental_rate DESC
```
### More than one column
```sql
SELECT title, special_features, rental_rate, name
 FROM film, film_category, category
WHERE film.film_id = film_category.film_id 
  AND film_category.category_id = category.category_id
ORDER BY rental_rate DESC, special_features ASC
```
## Using Limit
```sql
SELECT * FROM actor
LIMIT 10;
```

## Like

| Wildcard | Explanation                                                          |
|----------|----------------------------------------------------------------------|
| %        | Allows you to match any string of any length (including zero length) |
| _        | Allows you to match on a single character                            |

```sql
SELECT *
FROM film
WHERE special_features LIKE '%Trailers%'
```
When searching for characters _ %, they have to be escaped with \ (default escape character)

```sql
SELECT * FROM address
WHERE address LIKE '%\_%';
```
## Arithmetics
```sql
SELECT title, description, rental_rate * 15 AS "In Pesos" 
FROM film
```
