# MongoDB with NodeJS

At this stage of the process, you should have created a MongoDB Atlas account, created a cluster within your account, added a database to your cluster. With this setup complete, you may now start interacting with your database. 

The main ways to interact with your database are using the CRUD operations. CRUD stands for create, read, update, and delete. To create is to put one or more new entries in your database. To read is to retrieve one or more entries from your database. To update is to edit one or more entries from your database. To delete is to delete one or more entries from your database. Each entry is called a document.

The syntax for the CRUD operations is fairly simple and is explained below. Despite the simplicity, a few tips and tricks will help you master these basics. Before discussing each CRUD operation, first you should have a better understanding of MongoDB documents.

## [MongoDB Documents](#mongodb-documents)

Each entry in a MongoDB database is called a document. A document is a JSON-like object called a BSON. The BSON format generally looks and acts like JSON.

One of the main JSON-like features of a MongoDB document is that the structure of each document in a collection does not have to be identical to the structure of the other documents in that collection. For instance, if you have a users collection, some users may have a first name, last name, birthday, and user_id. However, not all users are required to have all the same fields. It's okay if some users are missing one or more of first name, last name, birthday, and user_id. This is different from SQL, which requires each entry (the SQL-equivalent of a document) in a table (the SQL-equivalent of a collection) to have all the same fields as the other entries in that same table.

Like the JSON format is comma-separated key:value pairs wrapped in curly braces, the BSON format has comma-separated field:value pairs wrapped in curly braces. One difference, however, is that BSON objects can store more data types than JSON. For instance, in a MongoDB document, you can store other documents and arrays of other documents. Here is an example from the [MongoDB documentation](https://docs.mongodb.com/manual/core/document/):

```
var mydoc = {
               _id: ObjectId("5099803df3f4948bd2f98391"),
               name: { first: "Alan", last: "Turing" },
               birth: new Date('Jun 23, 1912'),
               death: new Date('Jun 07, 1954'),
               contribs: [ "Turing machine", "Turing test", "Turingery" ],
               views : NumberLong(1250000)
            }
```

Here is the data type for each field in the `mydoc` object: 

- `_id` holds an ObjectId.
- `name` holds an embedded document that contains the fields first and last.
- `birth` and death hold values of the Date type.
- `contribs` holds an array of strings.
- `views` holds a value of the NumberLong type.

The `_id` field is a reserved field. Unless you add it yourself, MondgoDB automatically adds a unique `_id` value to each document created. It is a common practice to not add your own `_id` field but rather to let MongoDB add it for you.

The `_id` field will always be the first field in your document. Once it's set, you cannot change the `_id` value -- it's immutable. Although the `_id` value can be any data type other than an array, MongoDB defaults to an ObjectId data type when creating the `_id` for you. The `_id` is used as a primary key.

A few more tips about field names. Document field names are strings, cannot be or contain null, and top-level field names cannot start with a dollar sign ($).

MongoDB uses dot notation to access fields in an embedded document or items in an array. For instance, as explained in the [MongoDB documentation](https://docs.mongodb.com/manual/core/document/), the `name` field in the example above stores an embedded document. To access the `last` field in the embedded document, use dot notation like so `name.last`. 

Similar to embedded documents, for arrays use dot notation to access values. For instance, to access the third item in the array stored in the `contribs` field above, you write `"contribs.2"` because the third item is in the 2 index position. 

Using dot notation with embedded documents and arrays becomes relevant when you start writing query strings to read, update, and delete documents. For now, you should just know that this exists.

## [Create](#create)

Two main options exist for adding entries to your database. You can call a method that adds one document to a collection `.insertOne()` or you can call a method that adds multiple documents to a collection `insertMany()`. For example, if you have a collection named `users_collection`, you can use `.insertOne()` to add `userObject` to it, where `userObject` is one object. You can use `insertMany()` to add `multipleUsersObject` to it, where `multipleUsersObject` is an array of objects:

```node
const { MongoClient } = require("mongodb");
const url = "connection_string"; // Replace with your Atlas connection string
const client = new MongoClient(url);
const db = client.db("my_website");

// Use the collection named "users"
const users_collection = db.collection("users");

const userObject = {
    first: "Jane",
    last: "Doe",
  }
  
const multipleUsersObject = [
    {
      first: "Jane",
      last: "Doe",
    },
    {
      first: "John",
      last: "Doe",
    },
    {
      first: "Jack",
      last: "Hill",
    },
    {
      first: "Jill",
      last: "Hill",
    }
  ]


db.users_collection.insertOne(userObject)
db.users_collection.insertMany(multipleUsersObject)
```

When using either insert method, if the collection that you're inserting into does not yet exist, MongoDB will create the collection and also add the document(s) to it. If the collection already exists in the database, MongoDB will just add the document to it.

The insert methods each return a promise. Inside of the promise, you can access the `_id` using either the `insertedId` (for `insertOne()`) or `insertedIds` (for `insertMany()`). To see the code above in action, you can run the following script. It's the same as above but it also console logs the results of each insert method.

```node
const { MongoClient } = require("mongodb");

// Replace the following with your Atlas connection string
const url = "connection_string";
const client = new MongoClient(url);

// The database to use
const dbName = "test";

async function run() {
  try {
    await client.connect();
    console.log("Connected correctly to server");
    const db = client.db(dbName);

    // Use the collection named "users"
    const users_collection = db.collection("users");

    const userObject = {
      first: "Jane",
      last: "Doe",
    };

    const multipleUsersObject = [
      {
        first: "Jane",
        last: "Doe",
      },
      {
        first: "John",
        last: "Doe",
      },
      {
        first: "Jack",
        last: "Hill",
      },
      {
        first: "Jill",
        last: "Hill",
      },
    ];

    newUserDB = await users_collection.insertOne(userObject);
    multiplUsersDB = await users_collection.insertMany(multipleUsersObject);

    console.log("one result", newUserDB);
    console.log("multiple result", multiplUsersDB);

    
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

The console prints for each result a large connection object. Look through it to see what information is available to you. 

One field available in the result is the `_id` for the objects you insert. In the result objects, you should see for `insertOne()` the `insertedId` and for `insertMany()` the `insertedIds`. If instead of console logging the result like above, you instead console log the `insertOne()` result's `insertedId` and the `insertMany()` result's `insertedIds`, then the console prints the `_id` for `insertOne()` and an object containing `_ids` for `insertMany()`.

```node
console.log("one id-->", newUserDB.insertedId);
console.log("multiple ids-->",multiplUsersDB.insertedIds);

one id--> 5fa406ec3f4b71a70ae05dab
multiple ids--> {
  '0': 5fa406ec3f4b71a70ae05dac,
  '1': 5fa406ec3f4b71a70ae05dad,
  '2': 5fa406ec3f4b71a70ae05dae,
  '3': 5fa406ec3f4b71a70ae05daf
}
```

Another field available in the result is the `ops` field, which is the inserted document. In the result objects, you should see for both `insertOne()` and `insertMany()` an `ops` field. If instead of console logging the result or `insertedIds` like above, you instead console log the result's `ops` field, then the console prints an array of documents. Notice that the printed documents are the same as what you inserted except the inserted copies have an `_id` field, whereas the object in your code doesn't. That's because MongoDB insererted the `_id` for you.

```node
console.log("one id-->", newUserDB.ops);
console.log("multiple ids-->",multiplUsersDB.ops);

one id--> [ { first: 'Jane', last: 'Doe', _id: 5fa406ec3f4b71a70ae05dab } ]
multiple ids--> [
    { first: 'Jane', last: 'Doe', _id: 5fa406ec3f4b71a70ae05dac },
    { first: 'John', last: 'Doe', _id: 5fa406ec3f4b71a70ae05dad },
    { first: 'Jack', last: 'Hill', _id: 5fa406ec3f4b71a70ae05dae },
    { first: 'Jill', last: 'Hill', _id: 5fa406ec3f4b71a70ae05daf }
  ]
```

## [Read](#read)

Two main options exist for reading entries from your database. You can call a method that retrieves one document from a collection `.findOne()` or you can call a method that retrieves more than one document `find()`. For example, from your collection named `users_collection`, you can use `.findOne()` to get a specific document by passing into the method details about the document you want to get. In contrast, you can use `.find()` to get multiple documents that match the details that you pass into the method when calling it.

### .find()

Starting with the `.find()` method to get all documents from the collection. The `.find()` method accepts two optional arguments `.find(query, projection)`. The first argument is a query. The query is the filter you can use to specify which documents you want to retrieve. To return all documents in a collection, you can pass an empty document `.find({})` or leave the argument blank.  The second argument is the projection. The projection declares the fields to return for each document that matches the query. If you want all the fields for each document, leave this argument blank.

In the next chapter, you will practice using the query and projection parameters. For now, however, you should focus on using `.find()` with no arguments. 

Here is an example of using `.find()` with no arguments for retrieving all the documents from your `users_collection`. The code example below is very similar to the code you've seen above. The lines in the code below that you should focus on are `all_db_users = await users_collection.find();` and ` all_db_users.forEach((user) => console.log(user));`.

```node
const { MongoClient } = require("mongodb");

// Replace the following with your Atlas connection string
const url = "connection_string"; // Replace with your Atlas connection string
const client = new MongoClient(url);

// The database to use
const dbName = "test";

async function run() {
  try {
    await client.connect();
    console.log("Connected correctly to server");
    const db = client.db(dbName);

    // Use the collection named "users"
    const users_collection = db.collection("users");

    // Get all users
    all_db_users = await users_collection.find();

    // Print each user to the console
    all_db_users.forEach((user) => console.log(user));
    
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

The line `all_db_users = await users_collection.find();` retrieves all the documents from your `users_collection`. Notice that you pass no arguments into the `.find()` method. You would get the same result if you passed an empty object into the method `all_db_users = await users_collection.find({});`. 
 
The find method returns a cursor, which is a collection of documents. Console log the cursor (here it is named `all_db_users`) and look at the output, which a long object that won't be of much use to you now. 

Instead, what is of use to you now, you can iterate through the cursor to access each document in the cursor collection. Here's how to: `all_db_users.forEach((user) => console.log(user));`. This line of code should output the following in your console:

```node
{ _id: 5fa406ec3f4b71a70ae05dab, first: 'Jane', last: 'Doe' }
{ _id: 5fa406ec3f4b71a70ae05dac, first: 'Jane', last: 'Doe' }
{ _id: 5fa406ec3f4b71a70ae05dad, first: 'John', last: 'Doe' }
{ _id: 5fa406ec3f4b71a70ae05dae, first: 'Jack', last: 'Hill' }
{ _id: 5fa406ec3f4b71a70ae05daf, first: 'Jill', last: 'Hill' }
```

Try something else. Instead of console logging each user, try console logging just their first names `all_db_users.forEach((user) => console.log(user.first));`. 
You should see the console print the following: 

```node
Jane
Jane
John
Jack
Jill
```

This dot notation should be familiar to you. You use it to access values inside an object. What data type is the `user`? Check the data type of the each document in the cursor `all_db_users.forEach((user) => console.log(typeof user));`. The console should print:

```node
object
object
object
object
object
```
### .findOne()

The `.findOne()` method finds only one document from the collection. Like the `.find()` method, the `.findOne()` method accepts two optional arguments -- a query and a projection `.findOne(query, projection)`. If more than one document matches the query, the `.findOne()` method returns the first document that matches the query, which is usually the most recently inserted document that matches the query.

Although the `.find()` and `.findOne()` methods have the same parameters (optional query and optional projection), they return different things. As you saw above, the `.find()` method returns a cursor, which is a collection of documents. In contrast, the `findOne()` method returns a single document (not a collection of them). 

Here is an example of using `findOne()` with no arguments for retriecing one document from your `users_collection`. The code example below is the same as the previous example. The lines in the code below that you should focus on are `one_db_user = await users_collection.findOne();` and `console.log(one_db_user);`.

```node
const { MongoClient } = require("mongodb");

// Replace the following with your Atlas connection string
const url = "connection_string"; // Replace with your Atlas connection string
const client = new MongoClient(url);

// The database to use
const dbName = "test";

async function run() {
  try {
    await client.connect();
    console.log("Connected correctly to server");
    const db = client.db(dbName);

    // Use the collection named "users"
    const users_collection = db.collection("users");

    one_db_user = await users_collection.findOne();

    console.log(one_db_user);
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

The variable `one_db_user` stores a single document. Because you didn't pass a query argument into the `findOne()` method, the method returns the most recently inserted document. The line `console.log(one_db_user);` prints that document to the console, like so below.

```node
{ _id: 5fa406ec3f4b71a70ae05dab, first: 'Jane', last: 'Doe' }
```

Like you did for the `.find()` method above, you can console log a specific value in the returned document using dot notation `console.log(one_db_user.first);` and also console log the `typeof` the document returned `console.log(typeof one_db_user);`.

Although you may at times want to get only the most recently inserted document, it is just as (if not more) likely that you will want to get a document based on other criteria. Although later you will learn about this in more detail, here is an example of using the query argument to get a document that has a specific `_id`.

A very common occurrence in web applications is to get from your databse a user (or other specific item) by it's unique id. Using the `.findOne()` method is a great way to do this. However, you need to pay attention to a at least one nuance to avoid confusing errors for what seems to be a simple action. 

In MongoDB, the data type for `_id` is ObjectID (notice that `ID` has both letters capitalized; it's not `Id`). NodeJS, however, doesn't have the Mongo ObjectID data type built into it. You therefore need to import it from MongoDB and pass as a string into the ObjectID class to conver the string to an ObjectID. Here is an exmample. 

```node
const { MongoClient, ObjectID } = require("mongodb");

// Replace the following with your Atlas connection string
const url = "connection_string"; // Replace with your Atlas connection string
const client = new MongoClient(url);

// The database to use
const dbName = "test";

async function run() {
  try {
    await client.connect();
    console.log("Connected correctly to server");
    const db = client.db(dbName);

    // Use the collection named "users"
    const users_collection = db.collection("users");

    one_db_user = await users_collection.findOne({
      _id: ObjectID("5fa406ec3f4b71a70ae05dab"),
    });

    console.log(one_db_user);
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

The code above is similar to the previous examples. One difference in this example is the very first line, which is now `const { MongoClient, ObjectID } = require("mongodb");`. This line now imports `ObjectID` from the mongodb package you installed. 

Another difference is in the `try` block. It's this line.

```node 
one_db_user = await users_collection.findOne({
      _id: ObjectID("5fa406ec3f4b71a70ae05dab"),
    });   
```

This is the same line as the previous example `one_db_user = await users_collection.findOne();`, except this time you pass into the `.findOne()` method an object that specifies a key `_id` and value `ObjectID("5fa406ec3f4b71a70ae05dab")` by which the `.findOne()` method will filter. In other words, the method will look for the document that has an `_id` of `ObjectID("5fa406ec3f4b71a70ae05dab")`. Notice that it is a string string passed into the `ObjectID` class. The code example above logs the following to the console.

```node 
{ _id: 5fa406ec3f4b71a70ae05dab, first: 'Jane', last: 'Doe' }
```

If the method doesn't find a document matching the query filter, the method returns `null`.

## [Update](#update)

Three main options exist for updating entries in your database. You can call a method that updates one document from a collection `.updateOne()`, you can call a method that updates more than one document `.updateMany()`, or you can call a method that replaces one document `.replaceOne()` except the `_id` field. You typically should use `.updateOne()` and `.updateMany()` when updating less than all of the values in the document(s). In contrast, you should use .`replaceOne()` when updating all the values (except the `_id`).

### .updateOne()

Starting with the `.updateOne()` method to update values in one document. The `.updateOne()` method accepts two required arguments (filter and update) and one optional (options) `.updateOne(filter, update, options)`. The first argument is a filter. The filter is like the query argument used for the `find` methods. You use filter to specify which document to update. For instance, you can specify an empty object `.updateOne({})` to update the first document in the collection. Or you can specify specific criteria, like the `_id` of a document `.updateOne({_id: ObjectID("5fa406ec3f4b71a70ae05dab")})`, to target a specific document.

The second argument is the update. The update declares the updates that you want to make in the document. Use the [update operator expressions](https://docs.mongodb.com/manual/reference/operator/update/#id1) to update the document. While all of the update operators might help you, pay special attention to the `$set` operator. It is the one you will likely use most.

```node
Name	Description
$currentDate	Sets the value of a field to current date, either as a Date or a Timestamp.
$inc	Increments the value of the field by the specified amount.
$min	Only updates the field if the specified value is less than the existing field value.
$max	Only updates the field if the specified value is greater than the existing field value.
$mul	Multiplies the value of the field by the specified amount.
$rename	Renames a field.
$set	Sets the value of a field in a document.
$setOnInsert	Sets the value of a field if an update results in an insert of a document. Has no effect on update operations that modify existing documents.
$unset	Removes the specified field from a document.
```

For now, don't worry about the options argument. It is optional, so you can simply just omit it when calling the method.

Here is an example of using `.updateOne()` with arguments for filter and updates. The options argument is omitted. The code example below is very similar to the code you've seen above. The lines in the code below that you should focus on are `updated_user = await users_collection.updateOne({_id: ObjectID("5fa406ec3f4b71a70ae05dab"),}, { $set: { first: "enaJ" } });.` 

```node
const { MongoClient, ObjectID } = require("mongodb");

// Replace the following with your Atlas connection string
const url =
  "mongodb+srv://User1:test@cluster0.n5f5h.mongodb.net/<dbname>?retryWrites=true&w=majority";
const client = new MongoClient(url);

// The database to use
const dbName = "test";

async function run() {
  try {
    await client.connect();
    console.log("Connected correctly to server");
    const db = client.db(dbName);

    // Use the collection named "users"
    const users_collection = db.collection("users");

    updated_user = await users_collection.updateOne(
      {
        _id: ObjectID("5fa406ec3f4b71a70ae05dab"),
      },
      { $set: { first: "enaJ" } }
    );
    console.log(updated_user);

    one_db_user = await users_collection.findOne({
      _id: ObjectID("5fa406ec3f4b71a70ae05dab"),
    });
    console.log(one_db_user);
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

The example above uses the .`updateOne()` method to update the document having an `_id` of `ObjectID("5fa406ec3f4b71a70ae05dab")` by changing the value of `first` from `Jane` to `enaJ`. The code then console logs the return value `updated_user`. Look through that return value in your console. It has a lot of information about the update, some of which you may want to use to help make your application more robust. 

After console logging the returned update object, the example then uses the `.findOne()` method to get the same document so that you can console log it and see that the update did in fact occur. The second console log (`console.log(one_db_user)`) prints `{ _id: 5fa406ec3f4b71a70ae05dab, first: 'enaJ', last: 'Doe' }`. Update successful!

### .updateMany()

### .replaceOne()

## [Delete](#delete)
