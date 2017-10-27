## Intro to NoSQL - Martin Fowler
Can be found [https://www.youtube.com/watch?v=qI_g07C_Q5I](here). If you haven't seen this, you should definitely watch to get an overview.

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

### Explanations

1. Key Value store
	1. We just need a key to retrieve. Can be anything stored as a value.
	2. Can be anything in the DB. Can be an image, DB, a number or an object or anything.
  	3. Simply said, its like a hashmap.

2. Document DB
	1. It stores info as documents. What format should the document be is a natural question?
  	2. It can be in JSON or XML.
  	3. So, we can query the DB so that it actually extracts these fields from the JSON docs.

3. Column family DB
	1. There is a unique row ID for one particular object which identifies the entire obj. Then for each fields, there are column IDs. Each of the column IDs correspond to different fields of the object.
	2. <Should read more>

### Wow! Seems like NoSQL is awesome. but is it though??
1. When objects are simply retrieved as such, it is no problem to scale and clusterize. Are there any problems with this approach?
2. yes there are a few problems. NoSQL uses an aggregate style which is mapped on storage. Eg. The objects are usually stored per user basis. like, all the orders, invoices, products and history are stored as belonging to one person.
3. The problem arises when, we want to do an analysis of all the orders of the users - like - determining the orders cost done this year and last year. ** It is a disadvantage if we want to slice and dice the data in different ways**

### Aggregate DB style 
1. So, looks like the above 3 database structures can be classified into a supertype of DB structure called as Aggregate Databases.
2. Aggregate style is the heart of the following no sql db styles  
	1. Key value style
	2. Document DB
	3. Column Family

3. The third and the outlier which does not belong here is the **Graph databases**.


* Relational Databases - update based on ACID properties. Whereas NoSQL db update based on BASE (~~Should see what this is later~~).

### CAP theorem
* In case of a network partition, we have to choose between either Consistency or Availability. We **CANNOT** have both. 
