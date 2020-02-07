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