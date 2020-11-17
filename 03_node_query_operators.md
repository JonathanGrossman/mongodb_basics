# Query Operators

Query operators help you find, update, and delete documents. So far, you know how to query a single document or all documents. 

For instance, to target a specific document, you can pass an object as an argumet into the `.findOne()` method that finds a specific document by its `_id`. 

```node
one_db_user = await users_collection.findOne({
      _id: ObjectID("5fa406ec3f4b71a70ae05dab"),
    });
```

You've also seen code that passes an object (an empty object) as an argument that finds all documents. 

```node
all_db_users = await users_collection.find({});
```

But what if you want to write more powerful search queries that allow you to do more than get a single document by its `_id` or get less than all documents? 

This chapter discusses slighlty more advanced techniques for writing queries. Specifically, you will learn how to pass objects as arguments that use comparison and logical operators so that you can receive a subset of documents from your collection that match the criteria.

When working with databases, it may be tempting to retrieve all documents from a collection and then filter the results in your server-side or client-side code. Although that may be an appropriate solution for some situations, you will need in other situations to filter what you get back from the database instead of filtering after. 

The follow sections explain how to use comparison operators and logical operators to craft targeted database queries.

## [List of comparison operators](#list-of-comparison-operators)

Comparison operators compare two or more values. Examples of comparison operators are greater than, less than, greater than or equal to, and less than or equal to. You should be familiar with comparison operators from other programming languages. 

MongoDB comparison operators allow you to get a subset of documents based on a comparison. For instance, instead of getting one document by its `_id` or all documents from a collection, you can get only the documents that have a field with a value that meets the condition of your comparison operator. For instance, you can retrieve only the documents that have a field named `score` with a value greater than `10`. Or you can get documents that have a field named `first_name` with a value of `'Jane'`. 

Using comparison operators in MongoDB is similar to but different from using comparison operators in other programming languages. The similarities are conceptual. For instance, greater than is greater than. Less than is less than. 

Some key differences are syntax and order of operations. MongoDB uses syntax like `$gt` and `$lt`, whereas languages like JavaScript use mathematical symbols like `>` and `<`. Plus, you use comparison operators generally through your code in other languages, whereas in MongoDB they belong inside an object that serves as an argument inside a function call (e.g., `find({ score: { $eq: 10 } })`). 

In addition to syntax differences, another difference between comparison operators in MongoDB and other languages is the order of operations in MongoDB is not the same as for algebra. To understand the MongoDB comparison order, visit [MongoDB's page about comparison/sort order](#https://docs.mongodb.com/manual/reference/bson-type-comparison-order/#bson-types-comparison-order). Before worrying too much about order of operations, try practicing with simple examples.

Here is a list of MongoDB comparison operators.

`$eq`  Equal to: Matches values that are equal to a specified value.  
`$gt`  Greater than: Matches values that are greater than a specified value.  
`$gte`  Greater than or equal to: Matches values that are greater than or equal to a specified value.  
`$in`  In an array: Matches any of the values specified in an array.  
`$lt`  Less than: Matches values that are less than a specified value.  
`$lte`  Less than or equal to: Matches values that are less than or equal to a specified value.  
`$ne`  Not equal to: Matches all values that are not equal to a specified value.  
`$nin`  None: Matches none of the values specified in an array.  

Seeing some examples below will demonstrate.

## [Examples of comparison operators](#examples-of-comparison-operators)

Seeing a few examples of MongoDB comparison operators will demonstrate how they work. For the examples in this section, assume your database has the following documents in the users collection:

```node
{
  _id: 5fb3865234d40445e378b67f,
  first: 'Jane',
  last: 'Doe',
  languages: [ 'javascript', 'python' ],
  score: 3
}
{
  _id: 5fb3865234d40445e378b680,
  first: 'Jane',
  last: 'Doe',
  languages: [ 'javascript' ],
  score: 9
}
{
  _id: 5fb3865234d40445e378b681,
  first: 'John',
  last: 'Doe',
  languages: [ 'python', 'java' ],
  score: 10
}
{
  _id: 5fb3865234d40445e378b682,
  first: 'Jack',
  last: 'Hill',
  languages: [ 'java', 'ruby', 'javascript' ],
  score: 6
}
{
  _id: 5fb3865234d40445e378b683,
  first: 'Jill',
  last: 'Hill',
  languages: [ 'ruby' ],
  score: 8
}
```

### greater than

Starting with a simple 'greter than' comparison using the `$gt` operator. This operator matches documents where the value of a field is greater than the specified value. The following script uses the `.find()` method to retrieve only the documents that have a `score` field with a value greater than `5`. The example script below is simialr to examples you've seen in previous sections. The line to pay special attention to is `filtered_db_users = await users_collection.find({ score: { $gt: 5 } });`.

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

    filtered_db_users = await users_collection.find({ score: { $gt: 5 } });

    filtered_db_users.forEach((user) => {
      console.log(user);
    });
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

In the example above, the line `filtered_db_users = await users_collection.find({ score: { $gt: 5 } });` searches for documents that have a score greater than 5. Inside of the `.find()` method, the example passes an object as an argument. That object is `{ score: { $gt: 5 } }`. The object's key is `score`. This is the field that the `.find()` method looks for to do the comparison. 

The value for the `score` field in the object is another object `{ $gt: 5 }`. This object has the comparison operator of `$gt` as it's key and the number `5` as its value. 

If you console log `filtered_db_users`, you see in the console a large Cursor object. Review it to see if anything useful exists there for you. If, however, you loop through `filtered_db_users` and console log each item as you loop through (e.g., the forEach loop in the example above), you see each document returned from your query. 

```node 
{
  _id: 5fb3865234d40445e378b680,
  first: 'Jane',
  last: 'Doe',
  languages: [ 'javascript' ],
  score: 9
}
{
  _id: 5fb3865234d40445e378b681,
  first: 'John',
  last: 'Doe',
  languages: [ 'python', 'java' ],
  score: 10
}
{
  _id: 5fb3865234d40445e378b682,
  first: 'Jack',
  last: 'Hill',
  languages: [ 'java', 'ruby', 'javascript' ],
  score: 6
}
{
  _id: 5fb3865234d40445e378b683,
  first: 'Jill',
  last: 'Hill',
  languages: [ 'ruby' ],
  score: 8
}
```

Notice that the users collection of documents as a whole has 5 documents (see the documents at the top of this section). The `.find()` comparison query in the example above, however, returns only 4 documents. That's because 1 of the 5 documents in the users collection has a `score` of `3`. Because `3` is not greater than `5`, that document is not included in the return of the `.find()` method in the example above.

### in

Next, you'll use the `$in` operator to filter for documents that have a field containing a value that is an array. If the field's value is an array, then `$in` selects only the documents whose field holds an array that contains at least one element that matches a value in the specified array. 

The following script uses the `.find()` method to retrieve only the documents that have a `languages` field with a value of `'javascript`. The example script below is simialr to examples you've seen in previous sections. The line to pay special attention to is `filtered_db_users = await users_collection.find({languages: { $in: ["javascript"] },});`.

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

    filtered_db_users = await users_collection.find({
      languages: { $in: ["javascript"] },
    });

    filtered_db_users.forEach((user) => {
      console.log(user);
    });
  } catch (err) {
    console.log(err.stack);
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

In the example above, the line `filtered_db_users = await users_collection.find({languages: { $in: ["javascript"] }});` searches for documents that have a languages array containing 'javascript'. Inside of the `.find()` method, the example passes an object as an argument. That object is `{languages: { $in: ["javascript"] }`. The object's key is `languages`. This is the field that the `.find()` method looks for to do the comparison. 

The value for the `languages` field in the object is another object `{ $in: ["javascript"] }`. This object has the comparison operator of `$in` as it's key and an array `["javascript"]` as its value. Although the array contains only one item, you still need to need to include it within the array because otherwise you receive an error message.

If you console log `filtered_db_users`, you see in the console a large Cursor object. Review it to see if anything useful exists there for you. If, however, you loop through `filtered_db_users` and console log each item as you loop through (e.g., the forEach loop in the example above), you see each document returned from your query. 

```node 
{
  _id: 5fb3865234d40445e378b67f,
  first: 'Jane',
  last: 'Doe',
  languages: [ 'javascript', 'python' ],
  score: 3
}
{
  _id: 5fb3865234d40445e378b680,
  first: 'Jane',
  last: 'Doe',
  languages: [ 'javascript' ],
  score: 9
}
{
  _id: 5fb3865234d40445e378b682,
  first: 'Jack',
  last: 'Hill',
  languages: [ 'java', 'ruby', 'javascript' ],
  score: 6
}
```

Notice that the users collection of documents as a whole has 5 documents (see the documents at the top of this section). The `.find()` comparison query in the example above, however, returns only 3 documents. That's because 2 of the 5 documents in the users collection have a `langauges` field that does not contain 'javascript'.

Now try adding more items to the array in your comparison object `{ $in: ["javascript"] }`. For instance, what happens when you add 'python' `{ $in: ["javascript", "python"] }`

## [List of logical operators](#list-of-logical-operators)

Logical operators allow you to use simple logic to craft searches. The MongoDB logical operators are `and`, `not`, `or`, and `nor`. You should be familiar with logical operators from other programming languages. 

Like comparison operators, MongoDB logical operators allow you to get a subset of documents from a collection. Intead of using comparisons, you use logic. For instance, you can get from a collection a subset of documents that meet one condition `and` another condition. For instance, you can retrieve only the documents that have both a field named `score` with a value greater than `10` and also a field named `first_name` with a value of `'Jane'`.  

Or you can get a subset of documents that meet one condition `or` a different condition. For instance, you can retrieve only the documents that have either a field named `score` with a value greater than `10` or a field named `first_name` with a value of `'Jane'`.  

Using logical operators in MongoDB is similar to using logical operators in other programming languages. The main difference is syntax. Like for comparison operators, MongoDB uses syntax like `$and` and `$not`, whereas languages like JavaScript use other syntax like `&&` or `||`. 

Here is a list of MongoDB logical operators. 

`$and`  Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.  
`$not`  Inverts the effect of a query expression and returns documents that do not match the query expression.  
`$nor`  Joins query clauses with a logical NOR returns all documents that fail to match both clauses.  
`$or`  Joins query clauses with a logical OR returns all documents that match the conditions of either clause.  

You can combine logical operators with comparison operators to make some powerful queries. You can also use logical operators without comparisons. Regardless, seeing some examples below will demonstrate.

## [Examples of logical operators](#examples-of-logical-operators)

