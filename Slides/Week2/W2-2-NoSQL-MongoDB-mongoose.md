class: center, middle

## NodeJS
##### NoSQL MongoDB database and mongoose
#### TX00CR77-3001 / Spring 2017
#### Patrick Ausderau

---

# Contents

1. NoSQL
1. MongoDB
1. mongoose

---

# NoSQL

* NoSQL = "non SQL", "non relational" or "not only SQL"
* Contains about anything that is not only relational databases
  * Document databases like MongoDB
  * Key-value storages like Redis
  * Graph databases like OrientDB and Neo4j
  * Object databases like ObjectDB
  * Tabular databases like Hbase and BigTable

---

# MongoDB

* [Free & open source](https://github.com/mongodb/mongo) document-oriented database
  * First released in February 2009
  * Currently v3.x

__Lab 1__
* [Install](https://docs.mongodb.com/manual/administration/install-community/)
* [Shell access](https://docs.mongodb.com/v3.0/tutorial/getting-started-with-the-mongo-shell/) (eventually, some UI like [Robomongo](https://robomongo.org/))
* [Enable authentication](https://docs.mongodb.com/manual/tutorial/enable-authentication/)

---

# Main features

* Ad hoc queries
* Indexing
* High availability (replication)
* Supports sharding (scaling horizontally)
* File storage
* JavaScript execution
* Aggregation with map-reduce

---

# Basic structure

* Server has multiple databases
* Database has multiple collections
  * Like tables in relational database
  * Usually contains many related documents
  * Do not enforce schema on documents
  * Starting from 3.2 validations available for documents (checked on inserts and updates)
* Collection has multiple documents (rows)
  * JSON-like structure for collection of key-value pairs
  * Serialized and stored as BSON (Binary JSON)
* Document has multiple fields (columns)
  * Can have different fields from each other within the collection
  * Collection-wide unique identifier (12-byte, stored usually in _id field) can be manually set for document. If not set, one is generated

---

# Modeling data 

* Modeling the data is different from relational databases
  * Storage is cheap compared to computing time 
  * Duplicating data is acceptable
* Model data by user requirements
* Joins on write, not on read
* Optimize for most usual use case
* Example: Blog post documents stores tags (strings) and comments (objects with text, author etc.) within the blog document itself (with author, text, date, etc.)

---

# Mongoose

* "Elegant MongoDB object modeling for Node.js"
* Provides a schema-based solution to model data
* Built-in type casting, validation, query building, business logic hooks etc.
* Methods return a promise
  * All async operations return promises
  * Queries do return object with .then() but are not actual fully-fledged promises (use .exec() to obtain one)
  * For backwards-compatibility reasons, Mongoose returns [mpromise](https://www.npmjs.com/package/mpromise) promises by default. But will be removed in Mongoose 5 and will be replaced by ES6 promises, if available, and otherwise by [Bluebird promises](https://github.com/petkaantonov/bluebird)
  * Can already be changed to native ES6 promises

---

# Connecting

```javascript
const mongoose = require('mongoose');
mongoose.Promise = global.Promise; //ES6 Promise

mongoose.connect('mongodb://localhost:27017/test').then(() => {
  console.log('Connected successfully.');
}, err => {
  console.log('Connection to db failed: ' + err);
});
```
* In case connection is lost, Mongoose automatically reconnects and executes all commands in buffer
* Format for URL is: mongodb://[username:password@]host1[:port1][/[database]]

__Lab 2__
* In your [cats-api](https://github.com/gofore/node-training/tree/master/express#exercise-12), connect the  database and start the server (`app.listen`) only if the connection is successfull
* Of course: `npm install --save mongoose`

---

# Schema

* maps to a MongoDB collection and defines the shape of the documents

```javascript
const Schema = mongoose.Schema;

const blogSchema = new Schema({
  title:  String,
  author: String,
  body:   String,
  comments: [{ body: String, date: Date, author: String }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});
```
---

# Schema types

* [Schema](http://mongoosejs.com/docs/guide.html) supports basic [types](http://mongoosejs.com/docs/schematypes.html): 
  * String
  * Number
  * Date
  * Buffer
  * Boolean
  * Mixed
  * ObjectId
  * Array

---

# Lab 3

Define a schema for cat with following attributes:

* name (string)
* age (number)
  * If you want to train more with momentjs, go for: dob (Date)
* gender (string, male or female (hint: `enum`))
* color (string)
* weight (number)

---

# Model

* To use the schema, we need to create a model associated with it:

```javascript
const Blog = mongoose.model('Blog', blogSchema);
```

* Each instance of model is a document
* Only way to store and retrieve data to/from database

---

# CRUD

* To create a new document, call the [model](http://mongoosejs.com/docs/models.html) `create` method:

```javascript
Blog.create({ hidden: false }).then(post => {
  console.log(post.id);
});
```

* To modify existing documents, call the `update` method and the `remove` to delete
* Documents can be retreived using models' `find`, `findById`, `findOne`, or `where`: 

```javascript
// select *
Blog.find().exec().then((blogs) => {
  console.log(`Got ${blogs.length} blogs`);
});

// where
Blog.find({ hidden: false }).where('date').gt(oneYearAgo).exec().then(data => {
  console.log(data);
});
```

---

# Lab 4

* Create a [static](https://github.com/gofore/node-training/tree/master/express#serving-static-files) or with [template](https://github.com/gofore/node-training/tree/master/express#template-engines) a html form. On `POST /cats` create a new cat with the data that comes as [body](https://github.com/gofore/node-training/tree/master/express#request-body) argument.
* Respond to `GET /cats` with JSON containing all the male cats that have weight over 10kg and are older than 10 years.

---

# Instance methods

* Models have some built-in instance methods
* Models can also have custom [instance methods](http://mongoosejs.com/docs/guide.html#methods):

```javascript
blogSchema.methods.findFromSameDate = (cb) => {
  return this.model('Blog').find({ date: this.date }, cb);
};
var post1 = new Blog({ date: Date.now(), ... });

post1.findFromSameDate((err, posts) => {
  console.log(posts); // woof
});
```

* Built-in instance methods should not be overwritten

---

# Static methods

* [Static methods](http://mongoosejs.com/docs/guide.html#statics) can be used for example to perform more sophisticated lookups:

```javascript
blogSchema.statics.findByTitle = (title, cb) => {
  return this.find({ title: new RegExp(title, 'i') }, cb);
};

Blog.findByTitle('My title', (err, posts) => {
  console.log(posts);
});
```

---

# Query helpers

* [Query helpers](http://mongoosejs.com/docs/guide.html#query-helpers) allow chained query building:

```javascript
blogSchema.query.byTitle = (title) => {
  return this.find({ title: new RegExp(title, 'i') });
};

Blog.find().byTitle('My title').exec((err, posts) => {
  console.log(posts);
});
```    

---
--- 
 
# Sources

* [NoSQL (wikipedia)](https://en.wikipedia.org/wiki/NoSQL)
* [MongoDB](https://www.mongodb.com/)
* [mongoose](http://mongoosejs.com/index.html)
