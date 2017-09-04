# Use MongoDB  
The documentation of mongo is very easy, clear and complete.  Please, see this in [official website](https://docs.mongodb.com/manual)

## How to use mongo for CRUD operations?
Documentation for CRUD Operations in https://docs.mongodb.com/manual/crud/   

### Exercises: 

> ** Copy and paste of another website **

 **Create, Read, Update, Delete**

Use this dataset: https://raw.githubusercontent.com/jasonmimick/learn-mongodb/master/sampledata.zip

This exercise will help you practice your ability to query and mondify data in MongoDB.

Start a shell connecting to:

And answer the following questions about the ``students.scores`` collection.

1. How many scores are there?
2. What are the different``kind``’s of scores?
3. How many of each kind of score does each student have?
4. How many students got at least a 90 on their exam?
5. How many students got less than a 60 on their quiz?
6. Create a copy of all the scores in another collection to use later, then update all the 
students who got less than a 60 on any kind of score with a new field, ``needsHelp`` : ``true``
7. Find the student with the lowest number which has the lowest overall quiz score 
and then delete all the scores for that student - he was expelled.
8. A new student transfered into your school, add them to the scores collection. (Make up values for 
the the new students score's on each kind of score.)


Embedded Documents and Field Order
-

Switch over to the ``fruits`` collection and inspect the ``apples`` collection.
Find all the apples which have color=green and taste=sweet-tart tags.

Array fun
-

MongoDB documents can have properties which are arrays. This is a powerful feature, but you need to practice caution 
when quering to make sure your results are valid.

Switch over to the ``stuff`` database and inspect the ``elems`` collection.
If you want to find the ``elems`` with ``“a” = { “b”:1,”c”:4 }``
you might try this query ``db.elems.find( { "a.b" : 1, "a.c" : 4 } )``.

Do the results look correct?

Aggregation Framework
-
How do I group and perform operations on query results in MongoDB? The Aggregation Framework.

Back to the ``students.scores`` namespace.

What is and which student got the highest exam score and the lowest quiz score?


