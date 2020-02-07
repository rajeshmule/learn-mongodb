## MongoDB
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
#### Features of mongodb:
  - supports ad hoc queries // you can search by field, range query and it also supports regular expression searches.
  - supports indexing
  - supports replication
  - schemaless database written in c++
  - supports json data model

#### Installation 
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
#### Shell commands
  - db.version()
  - db.stats()
  - db.help()

Few common operations are: 
#### List databases
`show dbs` lists all databases.

#### Create database
`use DB_NAME` // use sample creates a database sample if not present, otherwise connects to that database.

To check the current databse connected to, use `db`.

#### Delete database
  - `use DB_NAME` command connects to that specific database.
  - `db.dropDatabase() deletes that database.

#### List collections in a database
Suppose we want to list all collections from sample database.
  - First we connect to that database using `use sample`.
  - For a list of all collections inside sample database we use `show collections`.
  - `db.getCollectionNames()` inside a database returns same result.

#### Create collection
Inside sample database
  - we can use `db.users.insert({name: 'abc'})` to create collection `users` in sample database with a single document having name field in it.

  - We can explicitly create a collection using `db.createCollection('users')` inside sample database.

#### capped collection
Capped collection is a fixed size collecction that automatically overwrites its oldest entries when it reaches its maximum size. If you specify true, you need to specify size(in bytes) parameter also.
`db.createCollection("posts", {capped: true, size: 4096})`

  - We can even add maximum number of documents to a capped collection using max field.
`db.createCollection("posts", {capped: true, size: 100000, max: 4000})`

  - Use the isCapped() method to determine if a collection is capped.
  `db.collection.isCapped()`

  - You can convert a non-capped collection to a capped collection with the convertToCapped command:`db.runCommand({"convertToCapped": "mycoll", size: 100000});`

  - The size parameter specifies the size of the capped collection in bytes.

  #### Drop collection
  Deletes a collection.
  - `db.COLLECTION_NAME.drop()` drops the whole collection with the contents. 
  - `db.COLLECTION_NAME.remove({})` drops all document from collection but collection remains.

  #### Rename collection
    Renames a collection.
    `db.COLLECTION_NAME.renameCollection(NEW_COLLECTION_NAME)` renames collection to newer collection name. 

  ### Q And A Basic Mongodb

  - create a database named `sports`.
  ```
       > use sports
  ```
  - list all databases present in local mongod server.
    ```
        > show dbs
    ```
  - create 3 collections named `cricket`, `football`, `TT` in sports databse.
  ```
     > db.createCollection("cricket")
     > db.createCollection("fotball")
     > db.createCollection("TT")
  ```
  - add multiple players in those collections which should have fields like `name`, `age` and `email` and `cost`.
   ```
    >db.cricket.insert({"name":"Rajehs", "age": "32", "email" : "rajesh@mule.com", "cost": "100000"})
  ````
  - list all collections in sports database.
  ```
    >show collections
  ```
  - rename `TT` collection to `tennis`.
   
  ````
    >db.TT.renameCollection("tabletanis")
  ````

  - create a capped collection called `khokho` which should have max 3 documents.
  Try inserting more than 3 and see what happens?


  - check whether a collection is capped or not?
  - drop all documents from `football` collection.
  ```
      >db.football.droup()
  ```
  - delete cricket collection completely.
   
  ```
      >db.cricket.droup()
  ```
  - delete sports database.
  ```
    >db.dropDatabase() 
    { "dropped" : "sports", "ok" : 1 }
  ```

## CRUD
CRUD is create, read, update and delete.
Mongodb have methods defined for creating a document, updating a record, querying a collection to fetch matched results and deleting a specific or all documents from a collection.

#### Create
`db.COLLECTION_NAME.insert` method is used to insert a document inside a collection.
It takes objects or arrays[for multiple inserts] with key value pair as only arguments.
```js


db.users.insert({name: 'suraj', email: 'abc@gmail.com'});
// During the insert, mongod will create the _id field and assign it a unique ObjectId value, as verified by the inserted document:

{ "_id" : ObjectId("5c88df5e217aec4256df232c"), "name" : "suraj", "email" : "abc@gmail.com" }
```
  - If you specify `_id` field , it will take that value but the _id value must be unique within the collection to avoid duplicate key error.

  - Other functions like `insertOne` and `insertMany` are also used. 

#### Read
Querying a collection for specific document or multiple document uses the form `db.COLLECTION_NAME.find()` or `findOne`. It takes the query as first argument and projection as second.

```js
// when query is blank or empty object
db.users.find() // returns all documents from users
db.users.find({_id: 1234}) // returns single document
db.users.find({}, {name: 1, _id: 0}) // returns all document with only name field 
```

We will learn more about querying in next section.

#### Update
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

#### Array Updates
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

#### Delete
  - `db.COLLCETION_NAME.drop()` is used to drop the entire collection and its documents from the database.

  - `db.COLLECTION_NAME.remove()` takes query object to be removed.
  ```js
  {_id: 23, title: 'intro to mongodb', tags: ['database', 'mongo']}
  // To remove above document
  db.articles.remove({_id: 23});
  ```
1. Create a database named `blog`.
  ```
  >use blog
  switched to db blog

  ```
2. Create a collection called 'articles'.

  ```
  >db.createCollection("articles")
  ```
3. Insert multiple documents(at least 3) into atricles. It should have fields
  - title as string
  - description as String
  - author as nested object
    - author should have
      - name
      - email
      - age
      - example author: {name: 'abc', email: 'abc@gmail', age: 25}
  - tags : Array of strings like ['html', 'css'] 

```js
// An article should look like
{
  _id: 'some_random_id',
  title: '',
  description: '',
  author: {
    name: '',
    email: '',
    age: ''
  },
  tags: ['js', 'mongo']
}

```
## Q & A CRUD


```

>db.blog.insertMany([{"title" : "mongodb", "discription": "learn mongodb from documation", "author" : {"name" : "rajesh mule", "email": "rajesh@mule.com","age" : "32"} , "tags": ["node", "mongodb"]}])


```

4. Find all the articles using `db.COLLECTION_NAME.find()`

```
>db.articles.find()
```
5. Find a document using _id field.
```
>db.articles.find(ObjectId("5e3d12d8f4fdd89a0c9d0e27"))
```

6. Find documents using title and author's name field.
  ```
  db.articles.findOne({"author.name" : "rajesh mule", "title" : "mongodb"})

  ```
7. Find doucment using a specific tag.
```
  >db.articles.find({"tags": "node"})

```

8. Update title of a document using its _id field.

```
>db.articles.findOneAndUpdate({"_id" : ObjectId("5e3d12d8f4fdd89a0c9d0e27")}, [{$set: {"title" : "learn mongodb"}}],{returnNewDocument: true})
```


9. Update a author's name using article's title.
```
>db.articles.findOneAndUpdate({"title" : "learn mongodb"}, [{$set: {"author.name" : "Raje Mule"}}],{returnNewDocument: true})
```

10. Add additional tag in a specific document.
```
>db.articles.update({"_id" : ObjectId("5e3d12d8f4fdd89a0c9d0e27")},{$push:{"tags":"html"}})

```
11. Increment an auhtor's age by 5.  
```
>db.articles.update({"_id" : ObjectId("5e3d12d8f4fdd89a0c9d0e27")},{$inc:{"author.age":5}})
```

12. Delete a document using _id field with `db.COLLECTION_NAME.remove()`.

```
>db.articles.findOneAndDelete({"_id" : ObjectId("5e3d12d8f4fdd89a0c9d0e27")})
```

## Mongo Query
We will be using `find` or `findOne` to query documents most of the time from a collection.

`db.COLLECTION_NAME.find(query, projection)` takes query as its first argument and projection as second.

  - query specifies selection filter using query operators. To return all documents in a collection, omit this parameter or pass an empty document ({}).

  - The projection parameter determines which fields are returned in the matching documents.

#### Equality Operator
Queries for document which matches the exact selection criteria.
```js
// returns document with name as specified
db.COLLECTION_NAME.find({name: "Derek Dawson"})
```
#### null
```js
// The { item : null } query matches documents that either contain the item field 
// whose value is null or that do not contain the item field.
db.COLLECTION_NAME.find({ scores: null });
```

#### $exist
```js
//This query matches documents that do not contain the item field:
find({ scores: {$exists: false}}) 
```

#### Comparison Operator
To find documents that match a set of selection criteria.
Comparison operators include `$gt`, `$lt`, `$or`, `$ne`, `$exists`.

1. $gt
  - It checks for an integer value and outputs document based on the comparison result.
  ```js
  // Outputs all document whose weight > 65.
  db.COLLECTION_NAME.find({weight: {$gt: 55}});
  ```
  - Similarily $lt, $lte(less than or equal), $gte(greater than or equal), $ne(not equal) operates.

2. Query for ranges using $gt and $lt operator.
  ```js
  //Returns all document where age > 18 and age < 50.
  db.COLLECTION_NAME.find({age: {$gte: 18, $lt: 50}})
  ```

3. $or
  - To find document which matches one of the multiple query which fits the selection criteria.
  ```js
  // Retrives all document whose age is either greater or equal to 20 or weight above 50.
  db.COLLECTION.find({$or: [{age: {$gte: 40}}, {weight: {$gt: 50}}]})
  ```
4. AND as well as OR operator
  ```js
  // Returns document which satisfies and + any of or conditions.
  db.COLLECTION_NAME.find({gender: 'Female', $or: [{age: {$gte: 20}}, {weight: {$gt: 47}}]})
  ```
#### Regex Operator
Matches a given pattern and returns document accordingly.

```js
// You're looking for documents that contains "m" somewhere in their name.
db.COLLECTION_NAME.find({name: /in/i})
```

#### Querying nested objects
Querying nested Object is similar to normal queries.
```js
// Suppose we have a document
{
  name: "",
  email: "",
  family: {
    father: "",
    mother: ""
  }
}
// In order to query using the family's mother field
db.COLLECTION_NAME.find({'family.mother': ""});
``` 

#### Querying array fileds 
Array is considered as first class citizen in mongo documents. We could add fields which could contain array of documents i.e array of integers, strings, arrays or objects.

1. Query all matched collection
```js
// Query all documents which contains a specific field in sports array
db.COLLECTION_NAME.find({sports: 'football'})
```
2. Exact match with order
```js
// query for document which contains exact match with specific order
db.COLLECTION_NAME.find({sports: ['cricket', 'football']})
```
3. Find all match without order($all)
```js
// returns documents which contain all fields but not in order
db.COLLECTION_NAME.find({sports: {$all: ['football', 'hockey']}})
```
4. $in operator
```js
// Retuns all document where sports array conatins one of $in operator fields.
db.COLLECTION_NAME.find({sports: { $in: ['khokho'] }}); 
```
5. Comparison
  - Using $gt or any other operator
  ```js
  // Retuns document where one element can satisfy the first condition and another 
  // element can satisfy the other condition, or a single element can satisfy both
  find({ scores: { $gt: 15, $lt: 20 } });
  ```
  - $elemMatch operator
    - Use $elemMatch operator to specify multiple criteria on the elements of an array such that at least one array element satisfies all the specified criteria.
    ```js
    // returns document if any element from scores array lies in between 22 and 30
    find({ scores : { $elemMatch: { $gt: 22, $lt: 30 } } });
    ```
    - querying array Indexes
      - query conditions for an element at a particular index or position of the array.
    ```js
    find({ 'scores.1': { $gt: 25 } });
    ```
    - query array by number of elements
      - Use the $size operator to query for arrays by number of elements.
    ```js
    find({ scores: { $size: 3 }})
    ```

## INDEX
Indexes are an efficient way to lookup data by its value.
Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures that store a small portion of the collection’s data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. 

#### Single Field Index
In order to query a document efficiently, we create index on that field.
We provide order as `1 or -1` for creating ascending or descending order for indxes.

```js
{
    _id: 123,
    state: 'delhi',
    area: 1334424
}
// Create a index on state field
db.COLLECTION_NAME.createIndex({state: 1})
```
#### Unique Index on a Single Field
A unique index ensures that the indexed fields do not store duplicate values; i.e. enforces uniqueness for the indexed fields.

MongoDB creates a unique index on the _id field during the creation of a collection. The _id index prevents clients from inserting two documents with the same value for the _id field. You cannot drop this index on the _id field.

To create a unique index, use the `db.collection.createIndex()` method with the unique option set to true.
```js
db.COLLECTION_NAME.createIndex( { "email": 1 }, { unique: true } )
```
You can also enforce a unique constraint on compound indexes. If you use the unique constraint on a compound index, then MongoDB will enforce uniqueness on the combination of the index key values.

#### Create a index on Embedded Field
```js
{
    name: '',
    address: {
        city: ""
    }
}
// Create a index on city field of address
db.COLLECTION_NAME.createIndex({"address.city": 1})
``` 
#### Compound Index
MongoDB supports compound indexes, where a single index structure holds references to multiple fields within a collection’s documents.
For example: 
```js
{
    name: "",
    state: "",
    city: ""
}
// We will create index on state and city
db.COLLECTION_NAME.createIndex({state: 1, city: 1}); // Now we cannot create duplicate cities for same state.
```
#### Multikey Index
To index a field that holds an array value, MongoDB creates an index key for each element in the array. These multikey indexes support efficient queries against array fields. Multikey indexes can be constructed over arrays that hold both scalar values(e.g. strings, numbers) and nested documents.

```js
// for documents
{ _id: 6, type: "food", item: "bbb", ratings: [ 5, 9 ] }
{ _id: 7, type: "food", item: "ccc", ratings: [ 9, 5, 8 ] }

// we can create multi field index for ratings field.
db.COLLECTION_NAME.createIndex({ratings: 1});

// We can query for specific documents
db.COLLECTION_NAME.find( { ratings: [5, 9] }); // Instead of scanning all documents, it will look using indexes for specific match.

```

#### Text Index
MongoDB provides text indexes to support text search queries on string content. text indexes can include any field whose value is a string or an array of string elements.

To create a text index, use the `db.collection.createIndex()` method. To index a field that contains a string or an array of string elements, include the field and specify the string literal "text" in the index document.
```js
db.COLLECTION_NAME.createIndex({ comments: "text" });

//You can index multiple fields for the text index. The following example creates a text index on the fields subject and comments
db.reviews.createIndex({ subject: "text", comments: "text" });

// Now we can search text fields using text search index.It will look for these text inside subject and comment fields.
db.reviews.find({$text:{$search:"any text"}})
```

#### List Indexes for a collection.
For listing all indexes on a collection:
```js
// List all indexes
db.COLLECTION_NAME.getIndexes()
``` 

#### Drop Indexes
`db.COLLECTION_NAME.dropIndex(index name)` is used to drop a index from a specific collection.
```js
// drop Single Index
db.COLLECTION_NAME.dropIndex("index name");

// drops all indexes except _id field index.
// Drops all indexes other than the required index on the _id field. Only call dropIndexes() as a method on a collection object.
db.COLLECTION_NAME.dropIndexes();
```

