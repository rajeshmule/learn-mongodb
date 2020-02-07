# Learn mongoDB
##MongoDB
MongoDB is a No SQL database. It is an open-source, cross-platform, document-oriented database written in C++. It uses javascript as their query language.

A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents.

```js
{
  title: '',
  decsription: '',
  tags: ['mongodb', 'node', 'js'],
  author: {
    name: '',
    email: ''
  }
}
```
### Features of mongodb:
  - supports ad hoc queries // you can search by field, range query and it also supports regular expression searches.
  - supports indexing
  - supports replication
  - schemaless database written in c++
  - supports json data model

## Installation 
Ubuntu: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/
MacOS: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/

 - check version using mongo --version

Once installed, start the mongodb server. It runs on default port `27017`.
Once server is started, we can run mongo scripts in shell by running `mongo` in terminal.

Mongo server can list all the databses present on local system. The structure of mongodb databases are:

  - We have databases at top level.
  - A database consists of list of collections.
  - A collection may contain multiple documents.
  - Each document can have 1 to multiple fields.

We can perform all database operations using mongo shell.
### Shell commands
  - db.version()
  - db.stats()
  - db.help()

Few common operations are: 
### List databases
`show dbs` lists all databases.

### Create database
`use DB_NAME` // use sample creates a database sample if not present, otherwise connects to that database.

To check the current databse connected to, use `db`.

### Delete database
  - `use DB_NAME` command connects to that specific database.
  - `db.dropDatabase() deletes that database.

### List collections in a database
Suppose we want to list all collections from sample database.
  - First we connect to that database using `use sample`.
  - For a list of all collections inside sample database we use `show collections`.
  - `db.getCollectionNames()` inside a database returns same result.

### Create collection
Inside sample database
  - we can use `db.users.insert({name: 'abc'})` to create collection `users` in sample database with a single document having name field in it.

  - We can explicitly create a collection using `db.createCollection('users')` inside sample database.

### capped collection
Capped collection is a fixed size collecction that automatically overwrites its oldest entries when it reaches its maximum size. If you specify true, you need to specify size(in bytes) parameter also.
`db.createCollection("posts", {capped: true, size: 4096})`

  - We can even add maximum number of documents to a capped collection using max field.
`db.createCollection("posts", {capped: true, size: 100000, max: 4000})`

  - Use the isCapped() method to determine if a collection is capped.
  `db.collection.isCapped()`

  - You can convert a non-capped collection to a capped collection with the convertToCapped command:`db.runCommand({"convertToCapped": "mycoll", size: 100000});`

  - The size parameter specifies the size of the capped collection in bytes.

### Drop collection
Deletes a collection.
  - `db.COLLECTION_NAME.drop()` drops the whole collection with the contents. 
  - `db.COLLECTION_NAME.remove({})` drops all document from collection but collection remains.

### Rename collection
Renames a collection.
`db.COLLECTION_NAME.renameCollection(NEW_COLLECTION_NAME)` renames collection to newer collection name.  

## CRUD
CRUD is create, read, update and delete.
Mongodb have methods defined for creating a document, updating a record, querying a collection to fetch matched results and deleting a specific or all documents from a collection.

### Create
`db.COLLECTION_NAME.insert` method is used to insert a document inside a collection.
It takes objects or arrays[for multiple inserts] with key value pair as only arguments.
```js


db.users.insert({name: 'suraj', email: 'abc@gmail.com'});
// During the insert, mongod will create the _id field and assign it a unique ObjectId value, as verified by the inserted document:

{ "_id" : ObjectId("5c88df5e217aec4256df232c"), "name" : "suraj", "email" : "abc@gmail.com" }
```
  - If you specify `_id` field , it will take that value but the _id value must be unique within the collection to avoid duplicate key error.

  - Other functions like `insertOne` and `insertMany` are also used. 

### Read
Querying a collection for specific document or multiple document uses the form `db.COLLECTION_NAME.find()` or `findOne`. It takes the query as first argument and projection as second.

```js
// when query is blank or empty object
db.users.find() // returns all documents from users
db.users.find({_id: 1234}) // returns single document
db.users.find({}, {name: 1, _id: 0}) // returns all document with only name field 
```

We will learn more about querying in next section.

### Update
Updating a document or multiple documents from a collection could range from a single field update to deeply nested or array updates.

Update includes single field update as well as replacing a document entirely.

Update takes 3 arguments
1. query for fetching specific document(Object)
2. new document for updation as second
  - either {} or {} with $set operator
3. options as third argument.
  - {upsert: true} // creates new if not available // default: false
  - {multi: true} // updates multiple document matching // default: false

```js
// $ set returns newer fields with older intact 
db.users.update({_id: 1234}, {$set : {gender: 'male'}})

// just object as update replaces older document
db.users.update({_id: 1234}, {gender: 'male'}) // It replaces old document and only _id and gender field is present in newer document.

```

Updating integer field is done with `$inc` field. $inc operator is used to increment or decrement integer value fields.
```js
// It increments age field by 2.
db.users.update({_id: 1234}, {$inc: {age: 2}})
```

Updating nested objects is similar to normal update.Suppose we have:
```js
// A user object is
{_id: 10, name: {first: 'Suraj', last: 'Kumar'}};
// for updating users last name
db.users.update({_id: 10}, {$set: {'name.last': 'singh'}});
// new object is
{_id: 10, name: {first: 'Suraj', last: 'singh'}};
```

### Array Updates
$push is used to update an existing array field with new entry.
```js
// Suppose we have an article document
{_id: 23, title: 'intro to mongodb', tags: ['database', 'mongo']}
// Adding single entry(node) to array of tags
db.articles.update({_id: 23}, {$push: {tags: 'node'}})
// Result is
{ "_id" : 23, "title" : "intro to mongodb", "tags" : [ "database", "mongo", "node" ] }


//Adding multiple tags in array
db.articles.update({_id: 23}, {$push: {tags: {$each: ['js', 'NoSQL']}}})
```

To remove elements from an array `$pull` is used.
```js
db.articles.update({_id: 23}, {$pull: {tags: 'node'}})
```
$pop is used to remove elements from either start or end of the array.


We have `updateOne` and `updateMany` methods as well.

### Delete
  - `db.COLLCETION_NAME.drop()` is used to drop the entire collection and its documents from the database.

  - `db.COLLECTION_NAME.remove()` takes query object to be removed.
  ```js
  {_id: 23, title: 'intro to mongodb', tags: ['database', 'mongo']}
  // To remove above document
  db.articles.remove({_id: 23});
  ```
