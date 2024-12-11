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
