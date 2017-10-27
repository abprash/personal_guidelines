## Intro to NoSQL - Martin Fowler

### History of Nosql
* SQL was standard in the past. They also had some problems, we assemble structures of business objects, into structurally different rows in multiple tables leading to a lot of awkward problems. It is called **impedance matching**. Complex business objects were stripped of their characteristics.
* Rel DBs were dominant until 2000s.
* Owing to the vertical infeasibility of simply buying bigger machines (since SQL requires a single node, people looked for alternatives and developed them.) It does not work well with a large grid of clusters.
* Google and Amazon developed Bigtable and DynamoDB resply.


### Where does this term NoSQL come from?
* Twitter hashtag to just set up a meetup :P

### Defn of NoSQL
* 

### Chars of NoSQL
1. Non relational
2. **Cluster friendly**
3. Schema-less
4. Open source
5. They came out of the 21st century web tech


### Divided into 4 basic types
1. Key value store
2. Graph type
3. Document
4. Column-family

* Explanations

1. Key Value store
  1. We just need a key to retrieve. Can be anything stored as a value.
  2. Can be anything in the DB. Can be an image, DB, a number or an object or anything.
  3. Simply said, its like a hashmap.

2. Document DB
  1. It stores info as documents. What format should the document be is a natural question?
  2. It can be in JSON or XML.
  3. So, we can query the DB so that it actually extracts these fields from the JSON docs.
