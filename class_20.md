# Exercises

We try translate class 4 exercises to mongodb. Please download sakila db json's from [here](http://static1.1.sqspcdn.com/static/f/359481/26067410/1427081957890/sakila.tgz?token=zFkfE4QSKgfurxrUF1lieRHKNS8%3D).

1. Show title and special_features of films that are PG-13
2. Get a list of all the different films duration.
3. Show title, rental_rate and replacement_cost of films that have replacement_cost from 20.00 up to 24.00
4. Show title, category and rating of films that have 'Behind the Scenes'   as special_features 
5. Show first name and last name of actors that acted in 'ZOOLANDER FICTION'
6. Show the address, city and country of the store with id 1
7. Show pair of film titles and rating of films that have the same rating.
8. Get all the films that are available in store id 2 and the manager first/last name of this store (the manager will appear in all the rows).

## Resultados

#### Para Reemplazar los tipo de datos (cast) y guardar algunos string como float

```javascript 
db.films.find().forEach( function (x) {
    x['Rental Duration'] = parseFloat(x['Rental Duration']);
    x['Replacement Cost'] = parseFloat(x['Replacement Cost']);
    x['Length'] = parseFloat(x['Length']);
    db.films.save(x);
});
```


## Show title and special_features of films that are PG-13

  > `db.films.find({Rating:'PG-13'}, {Title:1, 'Special Features':1})`

## Get a list of all the different films duration.
  > `db.films.distinct("Length")`
## Show title, rental_rate and replacement_cost of films that have replacement_cost from 20.00 up to 24.00
  > `db.films.find({Rating:'PG-13'}, {Title:1, 'Rental Duration':1})`
## Show title, category and rating of films that have 'Behind the Scenes'   as special_features 
  > `db.films.find({'Replacement Cost':{$gte:20.00,$lte:24.00}}, {Title:1, 'Rental Duration':1, 'Replacement Cost':1})`
## Show first name and last name of actors that acted in 'ZOOLANDER FICTION'
### One Query way
  > `db.films.find({Title:'ZOOLANDER FICTION'},{"Actors.First name":1, "Actors.Last name":1})`

### Logical way
``` javascript
db.films.find({Title:'ZOOLANDER FICTION'}).forEach((it)=> { 
    var doc = [];
      it.Actors.forEach((act)=>{
          doc.push({'First Name':act['First name'],'Last Name':act['Last name']});
      })
      printjson(doc);
});
```

##
db.stores.find({_id:1}, {Address:1, City:1, Country:1})

## 7
### Only one query way
 ```javsacript 
db.films.aggregate(
   [
     { $group : { _id : "$Rating", films: { $push: "$Title" } } }
   ]
)
``` 
### Logical way

``` javascript
var by_rating = {};
db.films.find({}).forEach((film)=> { 
    if (by_rating[film.Rating]==undefined){by_rating[film.Rating] = []}
    by_rating[film.Rating].push({'Title':film.Title,'Rating':film.Rating})
});
printjson(by_rating);
```

## Get all the films that are unavailable in store id 2 and the manager first/last name of this store (the manager will appear in all the rows).

``` javascript
var staff_store2_ids = []
var staff_store2_names = {}
var films_available = []
db.stores.find({_id:2}, {"Staff.staffId":1, "Staff.Last Name":1, "Staff.First Name":1, _id:0}).forEach(staff=>{
    var fn = staff.Staff[0]['First Name']
    var ln = staff.Staff[0]['Last Name']
    var id =staff.Staff[0].staffId.value
    staff_store2_ids.push(id)
    staff_store2_names[id] = {fn:fn,ln:ln}
})



var rented_id_pair_staff={}
db.customers.find({"Rentals": { $elemMatch: {'Return Date': null, staffId:{$in:staff_store2_ids}} }}, {'Rentals.$': 1}).forEach(res=>{
    //console.log(res.)
    res.Rentals.forEach(rent=>{
        rented_id_pair_staff[rent.filmId.value]=rent.staffId.value
    }) 
})

db.films.find({_id:{$in: Object.keys(rented_id_pair_staff)}}).forEach((it)=> { 
    films_available.push({film:it,  staff_data:staff_store2_names[it._id.value]})
});

console.log(films_available)
```





