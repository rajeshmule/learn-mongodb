1. Import data from json file to mongodb database using `mongoimport`
  - mongoimport --host <host_name> --username <user_name> --password <password> --db
  DB_NAME --collection COLLECTION_NAME --file cities.json(file location) --jsonArray
  (as array of jso data)

  - host, username and password are optional fields.

    - --host : hostname // defaults to localhost:27017
    - --username and --password: optional username and password for db user
    - --db: database name
    - --collection: Collection name
    - --file: file location
    - --jsonArray: Passed an array of json objects 

2. Export data from mongodb server to localsystem in json format using
 `mongoexport`command.
  - mongoexport --db state --collection cities --out ~/Desktop/states/city.json
   --jsonArray

3. Import from csv file
  - mongoimport -d DB_NAME -c COLLECTION_NAME --type csv --file India\ general\
   election\ results\ 2014.csv(file location) --headerline(including header)

#### Explain
The explain() method allows you to examine query plans. It’s an essential tool for
tuning MongoDB performance.

For almost all operations, there will be more than one way for MongoDB to retrieve 
the documents required. When MongoDB parses a statement, it must decide which approach
will be fastest. The process of determining this “optimal” path to the data is
referred to as query optimisation.

There are 3 scans mostly used: 
1. COLLSCAN
   —  which means the document was scanned without an index.

2. IXSCAN
   — the use of an index to find documents

3. SORT
   — the sorting of documents without an index.

The execution statistics are included in the executionStages section of the resulting
plan document like `db. messages.explain("executionStats")`.