# Mongo DB

It is a NoSQL Database Management System.

- Data is stored as single document
    - Data in document is stored as key value pair(JSON like format)
    ```js
    {
        name: "Owais",
        age: 26,
        cgpa: 8.0,
        fulltime: false
    }
    ```
- Collection is a group of documents.
```js
    [
        {
            name: "Owais",
            age: 26,
            cgpa: 8.0,
            fulltime: false
        },
        {
            name: "Amir",
            age: 28,
            cgpa: 6.0,
            fulltime: false
        },
        {
            name: "Asif",
            age: 25,
            cgpa: 7.0,
            fulltime: true
        }
    ]
```
- Databse is a group of colletions.

## Installation

### mongoDB shell(mongosh)

- Download from [Download mongosh](https://www.mongodb.com/try/download/shell)
- Unzip downloaded folder and extract it.
- Go to [foldername]/bin
- copy the path of mongosh file in bin folder and set it in environment variables(PATH and MongoSh keys).
- Install MongoDB extension in vs-code.

## Data Types in MongoDB

- `String`
- `Integer`
- `Double`
- `Boolean`
- `Array`
- `Document`
- `null`
- `Date Object` -> `new Date()`

## Show All Databases
```sh
test> show dbs
```

## Use and create  database
```sh
test> use school
```

## Create a collection
```sh
school> db.createCollection("Students")
```

`Students` is the name of collection.

## Show All Collections
```sh
test> show collections
```

## Drop a database

```sh
school> db.dropDatabase()
```

## Insert a document in collection
```sh
school> db.students.insertOne({name: "Owais", age: 26, cgpa: 8.0})
```

## Insert many documents in collection
```sh
school> db.students.insertMany([{name: "Amir", age: 25, cgpa: 7.5}, {name: "Shahid", age: 27, cgpa: 6.9}, {name: "Ubaid", age: 24, cgpa: 7.7}])
```

## Get all documents from collection
```sh
school> db.students.find()
```

## Sort documents

In ascending order
```sh
school> db.students.find().sort({name: 1})
```

In Descending order
```sh
school> db.students.find().sort({name: -1})
```

## Limit number of documents returned

```sh
school> db.students.find().limit(2)
```

## Filter data

```sh
school> db.students.find({name: "Owais"})
```

returns all students having name `"Owais"`

## Filter data with specific fields

```sh
school> db.students.find({name: "Owais"}, {name: true, cgpa: true})
```

returns all students having name `"Owais"` where each document will have `_id`. `name` and `cgpa` fields

## Update a document

### Set a field

```sh
school> db.students.updateOne({name: "Owais"}, {$set: {fulltime: false}})
```

Selects a document with the given `name` and then updates its `fulltime` field to `false`.

### Unset a field

```sh
school> db.students.updateOne({name: "Owais"}, {$unset: {fulltime: ""}})
```

Deletes `fulltime` field.

## Update many documents

```sh
school> db.students.updateMany({}, {$set: {fulltime: false}})
```

Every document will have `fulltime` field of value `false`

```sh
school> db.students.updateMany({fulltime: {$exists: false}}, {$set: {fulltime: false}})
```

If `fulltime` field does not exist in a `document` then add the field and set it to `true`

## Delete a document

```sh
school> db.students.deleteOne({name: "Owais"})
```

## Delete many documents

```sh
school> db.students.deleteMany({name: "Owais"})
```

Deletes every document with `name` field set to `"Owais"`

```sh
school> db.students.deleteMany({fullTime: {$exists: false}})
```

Deletes every document that does not have `fullTime` field

## Operators

### Not Equal To ($ne)

```sh
school> db.students.find({name: {$ne: "Owais"}})
```

Returns all documents with `name` not equal to `"Owais"`

### Less Than ($lt)

```sh
school> db.students.find({cgpa: {$lt: 28}})
```

Returns all documents that have `age` less than `28`

### Less Than Or Equal To ($lte)

```sh
school> db.students.find({cgpa: {$lte: 28}})
```

Returns all documents that have `age` less than or equal to `28`

### Greater Than ($gt)

```sh
school> db.students.find({cgpa: {$gt: 28}})
```

Returns all documents that have `age` greater than `28`

### Greater Than Or Equal To ($gte)

```sh
school> db.students.find({cgpa: {$gte: 28}})
```

Returns all documents that have `age` greater than or equal to `28`

### Combining Operators

```sh
school> db.students.find({cgpa: {$gte: 3, $lte: 4}})
```

### In Operator ($in)

```sh
school> db.students.find({name: {$in: ["Owais", "Asif", "Ubaid"]}})
```

Returns all documents that have `name` equal to any of the array element

### Nin Operator ($nin)

```sh
school> db.students.find({name: {$nin: ["Owais", "Asif", "Ubaid"]}})
```

Returns all documents that have `name` not equal to any of the array element

### And Operator ($and)

```sh
school> db.students.find({$and: [{fullTime: true}, {age: {$lt: 27}}]})
```

Returns all documents that have `fullTime` set to `true` and `age` is less than `27`

### Or Operator ($or)

```sh
school> db.students.find({$or: [{fullTime: true}, {age: {$lt: 27}}]})
```

Returns all documents that have `fullTime` set to `true` or `age` is less than `27`

### Nor Operator ($nor)

```sh
school> db.students.find({$nor: [{fullTime: true}, {age: {$lt: 27}}]})
```
Note: -> Both conditions have to be false

Returns all documents that have `fullTime` set to `false` and `age` is greater than or equal to`27`

### Not Operator ($not)

```sh
school> db.students.find({age: {$not: {$gte: 27}}})
```

Returns all documents that do not have `age` greater or equal to `27`

## Connect with express

```js
const express = require("express")
const {MongoClient} = require("mongodb")

let db

(async function() {
    const client = new MongoClient("mongodb://localhost:27017")
    await client.connect()
    console.log("Connected Successfully")
    db = client.db('school')
})()

const app = express()

app.use(express.json())

app.get('/', async (req, res) => {
    const students = await db.collection("students").find().toArray()
    res.json(students)
})

app.post('/', async (req, res) => {
    const result = await db.collection("students").insertOne(req.body)
    console.log(result.acknowledged)
})

app.put('/:id', async (req, res) => {
    const id = req.params.id
    const result = await db.collection("students").updateOne({_id: new ObjectId(id)}, {$set: {cgpa: req.body.cgpa}})
    console.log(result)
})
```

# Mongoose Notes

## Connecting to DB

```js
const mongoose = require("mongoose")

mongoose.connect("mongodb://127.0.0.1:27017/<Database Name>")

```

## Schema and model

Schema is the definition of structure that documents of a collect adhere.

```js
const mongoose = require("mongoose")

const familySchema = new mongoose.Schema({
    father: String,
    mother: String,
    siblings: Number
})

const userSchema = new mongoose.Schema({
    name: String,
    age: Number,
    fulltime: Boolean,
    createdAt: Date,
    subjects: [String],
    address: {
        state: String,
        zip: Number
    },
    bestFriend: mongoose.SchemaTypes.ObjectId,
    family: familySchema,
    email: {
        type: String,
        required: true,
        minLength: 10,
        validate: {
            validator: (value) => isValidEmail(value),
            message: (props) => `${props.value} is not a valid email`
        }
    },
    pincode: {
        type: Number,
        immutable: true
    },
    phone: {
        type: Number,
        minLength: 10,
        maxLength: 10
    }
})
```

Model creates a collection that will contain documents following structure of a schema.

```js
...
module.exports = mongoose.model("User", userSchema)
```

## Creating a document

```js
const user = new User({name: "...", age: 21})
user.save()
```

```js
const user = User.create({name: "...", age: 21})
```

## Update a document

```js
app.put("/:id", async(req, res) => {
    const id = req.params.id
    const user = await User.findById(id)
    user.age = req.body.age
    await user.save()
    res.json(user)
})
```
