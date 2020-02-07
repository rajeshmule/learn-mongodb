1. Create a database named `blog`.
2. Create a collection called 'articles'.
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

4. Find all the articles using `db.COLLECTION_NAME.find()`
5. Find a document using _id field.
6. Find documents using title and author's name field.
7. Find doucment using a specific tag.

8. Update title of a document using its _id field.
9. Update a author's name using article's title.
10. Add additional tag in a specific document.
11. Increment an auhtor's age by 5.  

12. Delete a document using _id field with `db.COLLECTION_NAME.remove()`.
