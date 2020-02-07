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
#### solution


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
