## What is Dimensional Model?
A dimensional model is a data structure technique optimized for Data warehousing tools. The concept of Dimensional Modelling was developed by Ralph Kimball and is comprised of "fact" and "dimension" tables.

A Dimensional model is designed to read, summarize, analyze numeric information like values, balances, counts, weights, etc. in a data warehouse. In contrast, relation models are optimized for addition, updating and deletion of data in a real-time Online Transaction System.

These dimensional and relational models have their unique way of data storage that has specific advantages.

For instance, in the relational mode, normalization and ER models reduce redundancy in data. On the contrary, dimensional model arranges data in such a way that it is easier to retrieve information and generate reports.

## Elements of Dimensional Data Model
### Fact
Facts are the measurements/metrics or facts from your business process. For a Sales business process, a measurement would be quarterly sales number

### Dimension
Dimension provides the context surrounding a business process event. In simple terms, they give who, what, where of a fact. In the Sales business process, for the fact quarterly sales number, dimensions would be

 - Who – Customer Names
 - Where – Location
 - What – Product Name

In other words, a dimension is a window to view information in the facts.

### Attributes
The Attributes are the various characteristics of the dimension.

In the Location dimension, the attributes can be

State
Country
Zipcode etc.
Attributes are used to search, filter, or classify facts. Dimension Tables contain Attributes

### Fact Table
A fact table is a primary table in a dimensional model.

A Fact Table contains

Measurements/facts
Foreign key to dimension table

### Dimension table
 - A dimension table contains dimensions of a fact.
 - They are joined to fact table via a foreign key.
 - Dimension tables are de-normalized tables.
 - The Dimension Attributes are the various columns in a dimension table
 - Dimensions offers descriptive characteristics of the facts with the help of their attributes
 - No set limit set for given for number of dimensions
 - The dimension can also contain one or more hierarchical relationships

## What is a Star Schema?

The star schema is the simplest type of Data Warehouse schema. It is known as star schema as its structure resembles a star. In the Star schema, the center of the star can have one fact tables and numbers of associated dimension tables. It is also known as Star Join Schema and is optimized for querying large data sets.

![star-schema](/uploads/cdd397bde5da812ad9c5499391506e2d/star-schema.png)

## Introduction to the PostgreSQL **CUBE**
PostgreSQL CUBE is a subclause of the GROUP BY clause. The CUBE allows you to generate multiple grouping sets.

The following query uses the CUBE subclause to generate multiple grouping sets:

```sql
SELECT
    brand,
    segment,
    SUM (quantity)
FROM
    sales
GROUP BY
    CUBE (brand, segment)
ORDER BY
    brand,
    segment;
```

![cube-example](/uploads/8c43996c5fcbd114ca66f0ba68aebb75/cube-example.png)

The following query performs a partial cube:

```sql
SELECT
    brand,
    segment,
    SUM (quantity)
FROM
    sales
GROUP BY
    brand,
    CUBE (segment)
ORDER BY
    brand,
    segment;
```

![partial-cube-example](/uploads/eb4d2c85b49a6588b0db51baee0d9d7b/partial-cube-example.png)

## Introduction to the PostgreSQL **ROLLUP**

The PostgreSQL ROLLUP is a subclause of the GROUP BY clause that offers a shorthand for defining multiple grouping sets.

Different from the CUBE subclause, ROLLUP does not generate all possible grouping sets based on the specified columns. It just makes a subset of those.

PostgreSQL ROLLUP examples
The following query uses the ROLLUP subclause to find the number of products sold by brand (subtotal) and by all brands and segments (total).

```sql
SELECT
    brand,
    segment,
    SUM (quantity)
FROM
    sales
GROUP BY
    ROLLUP (brand, segment)
ORDER BY
    brand,
    segment;
```

![rollup-example](/uploads/6f530cba362a01728d840761ecaa7de4/rollup-example.png)

The following statement performs a partial roll-up:

```
SELECT
    segment,
    brand,
    SUM (quantity)
FROM
    sales
GROUP BY
    segment,
    ROLLUP (brand)
ORDER BY
    segment,
    brand;
```

![partial-rollup-example](/uploads/75086271920c3d49d51834c0a65ca287/partial-rollup-example.png)


## Excersices

1. Retrieve Internet Sales Amount As Per Customer. In other words, we can say show the Detail of amount spent by customers during purchase from Internet.

2. View Internet Sales amount detail between year 2005 to 2008

3. View Internet Sales by product category and sub-category.

4. View Internet Sales and Freight Cost by product category, sub-category and product.

5. Retrieve only those products whose names begin with “A” and Internet sales amount <5000

6. What is sales amount in all the countries?? 

7. Retrieve all the products in descending order of their Internet sales amount of year 2007 

8. Generate a report with Internet Sales sub total, grand total per year and month.

9. Generate a report with the amount of "Pedals" and "Tires and Tubes" category of products in the inventory. Also with the amount of in and outs of each of them on the second half of the year 2006.

10. Generate a report with the amount of calls, automatic responses and issues raised by the call center operators. On working days during the morning shift, from the 20th working week until the end of the year 2007.