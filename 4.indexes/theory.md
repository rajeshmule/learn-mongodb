## INDEX
Indexes are an efficient way to lookup data by its value.
Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures that store a small portion of the collection’s data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. 

### Single Field Index
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

### Create a index on Embedded Field
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
### Compound Index
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
### Multikey Index
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

### Text Index
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
