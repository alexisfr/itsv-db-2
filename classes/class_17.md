###  [Indexes](class_17_1.md)

###  [MySQL Full-Text Search](class_17_2.md)
This is a nice feature to take into account.

#### Exercises
For all the exercises include the queries in the class file.

**Note**: to disable cache in order to get better comparisons you can run the following.
```sql
SET GLOBAL query_cache_size = 0;

```

1. Create two or three queries using **address** table in sakila db:
   *  include **postal_code** in where (try with in/not it operator) 
   * eventually join the table with city/country tables.  
   * measure execution time.
   * Then create an index for **postal_code** on **address** table.
   * measure execution time again and compare with the previous ones.
   * Explain the results

2. Run queries using **actor** table, searching for first and last name columns independently. Explain the differences and why is that happening?

3. Compare results finding text in the description on table film with **LIKE** and in the film_text using **MATCH** ... **AGAINST**. Explain the results.