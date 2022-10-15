# MongoDB

### collection = table , document is a single record

<br>  

## 1.Access to MongoDB in the shell / command line

```javascript
mongod
```

```javascript
mongo
```
## 2. Commands

show all databses
```javascript
show dbs
```
create databse
```javascript
use shopDB
```
show the curent databse you are
```javascript
db
```
insert
```javascript
db.products.insertOne({_id: 1, name: "Pen, price: 1.2})
```
show collections currently exist in the current database
```javascript
show collections
```
READ : retrieve all the documents hat are inside collection
```javascript
db.products.find()
```
query with conditions
```javascript
db.products.find({name: "Pencil"})
```

```javascript
db.products.find({price: {$gt:1}})
```
query return only specific fields
```javascript
db.products.find({_id: 1}, {name: 1, _id: 0})
```
Update
```javascript
db.products.updateOne({_id: 1}, {$set: {stock:32}})
```
Delete
```javascript
db.products.deleteOne({_id: 2})
```
Relationship between product document and reviews documents : one to many relationship
```javascript
db.products.insert({
_id:2, 
name: ""Pencil"", 
price: 1.3, 
stock: 43, 
review : [ 
{ authorName: ""Sally"", rating: 5, review: ""Best rubber ever"" }, 
{ authorName: ""John"", rating: 5, review: ""awesome"" } 
] } )
```
delete a databse
```javascript
use shopDB
```
then
```javascript
db.dropDatabase()
```
Delete collection
```javascript
db.products.drop()
```


# Mongoose
## 1. Setup mongoose
```javascript
npm install mongoose
```

```javascript
const mongoose = require('mongoose');
```

```javascript
mongoose.connect('mongodb://localhost:27017/fruitsDB',{useNewUrlParser:true});
```
## 2.Commands

Create a collection or table
```javascript
const fruitsSchema = new mongoose.Schema({
name: {
  type: String,
  required: [true, "Please check data entry, no name specified"]
},
rating: {
  type: Number,
  min : 1,
  max: 10
},
review: String
});
```

```javascript
const Fruit = mongoose.model("Fruit", fruitsSchema);
```
Create / insert
```javascript
const fruit = new Fruit ({
  name: "Apple",
  rating: 7,
  review: "Good"
});

fruit.save();
```

```javascript
const kiwi = new Fruit ({
  name: "Kiwi",
  rating: 7,
  review: "Good"
});

const orange = new Fruit ({
  name: "Orange",
  rating: 7,
  review: "Good"
});

const banana = new Fruit ({
  name: "Banana",
  rating: 7,
  review: "Good"
});

Fruit.insertMany([kiwi,orange,banana], function(err){
  if(err){
    console.lof(err);
  } else {
    console.log("success");
  }
});
```

Read
```javascript
Fruit.find(function(err, fruits){
  if (err) {
    console.log(err);
  } else {
    // mongoose.connection.close();
    fruits.forEach(function(fruitelement) {
    console.log(fruitelement.name);
    })

  }
});
```
Update
```javascript
Fruit.updateOne({_id: "62bc960a878e8a91200251c9"}, {name: "Durian"}, function(err){
  if (err) {
    console.log(err);

  } else {
    console.log("success");
  }
});
```
Delete
```javascript
Fruit.deleteOne({_id: "62bdb7e8b36edfc89fc7587c"}, function(err){
  if (err) {
    console.log(err);
  } else {
    console.log("success");
  }
})
```

```javascript
Fruit.deleteMany({name: "apple"}, function(err){
  if (err) {
    console.log(err);
  } else {
    console.log("success");
  }
})
```
## create relationships

```javascript
const personSchema = new mongoose.Schema({
name: String,
age: Number,
favouriteFruit : fruitsSchema
});

```

```javascript
const Person = mongoose.model("Person", personSchema);
```
SUBPART 1 : CREATE relationship from new document
```javascript
const pineapple = new Fruit ({
  name: "pineapple",
  score : 9,
  review : "Great"
});

pineapple.save();
```

```javascript
const person = new Person ({
  name: "Amy",
  age: 12,
  favouriteFruit: pineapple
});

person.save();
```

SUBPART 2 : CREATE relationship from existing document
```javascript
const abricot = new Fruit ({
  name: "abricot",
  score : 9,
  review : "Great"
});

abricot.save();
```

```javascript
Person.updateOne({name: "John"}, {favouriteFruit: abricot}, function(err){
  if (err) {
    console.log(err);

  } else {
    console.log("success");
  }
});
```

```javascript

```




