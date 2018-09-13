# Query REFERENCE

## Select All Documents in a Collection
db.films.find( {} )

## Specify Equality Condition

To specify equality conditions, use <field>:<value> expressions in the query filter document:
 > `{ <field1>: <value1>, ... }`

#### Specify Conditions Using Query Operators
 > `{ <field1>: { <operator1>: <value1> }, ... }`

#### Specify AND Conditions
A compound query can specify conditions for more than one field in the collection’s documents. Implicitly, a logical AND conjunction connects the clauses of a compound query so that the query selects the documents in the collection that match all the conditions.

 >`db.customers.find({'First Name':'MARY', 'Last Name':'SMITH'})`

#### Specify OR Conditions
Using the **$or** operator, you can specify a compound query that joins each clause with a logical OR conjunction so that the query selects the documents in the collection that match at least one condition.

 > `db.films.find( { $or: [ { Category: "Documentary" }, { Rating: { $lt: 30 } } ] } )`

#### Specify AND as well as OR Conditions
In the following example, the compound query document selects all documents in the collection where the film Category is Documentary and Length is equals 86 Or  Rating is less than 30.

 > `db.films.find( {Category: "Documentary", $or: [ { Length:'86' }, { Rating: { $lt: 30 } } ] } ) `

### Filter equal
* Example for comparations where  (equals)
 > `db.films.find({Length:86})`

* Example for comparations where  (less than...)
 > `db.films.find({Length: {$lt:86  }})`
* Example for comparations where  (less than or equals ...)
 > `db.films.find({Length: {$lte:86  }})`

* Example for comparations where  (grater than...)
 > `db.films.find({Length: {$gt:86  }})`
* Example for comparations where  (grater than or equals ...)
 > `db.films.find({Length: {$gte:86  }})`


### Filter likes

* Example for where like = 'MARY'  
 >`db.customers.find({'First Name':'MARY'})`

* With where like = "MAR%"  
>`db.customers.find({'First Name':/^MAR/})`

* With where like = "%ARY"    
>`db.customers.find({'First Name':/ARY$/})`

* With where like = "%AR%"  
>`db.customers.find({'First Name':/AR/})`

### In clause

> db.films.find({Rating: {$in:["G","PG"]}}) 

## Select one field
`db.films.find({}, {title:1})`
