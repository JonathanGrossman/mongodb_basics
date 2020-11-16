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

This chapter discusses advanced techniques for writing queries. Specifically, you will learn how to pass objects as arguments that use comparison and logical operators so that you can receive a subset of documents from your collection that match the criteria.

When working with databases, it may be tempting to retrieve all documents from a collection and then filter the results in your server-side or client-side code. Although that may be an appropriate solution for some situations, you wil need in other situations to filter what you get back from the database instead of filtering after. 

The follow sections explain how to use comparison operators and logical operators to craft targeted database queries.

## [List of comparison operators](#list-of-comparison-operators)

Comparison operators compare two or more values. Examples of comparison operators are greater than, less than, greater than or equal to, and less than or equal to. You should be familiar with comparison operators from other programming languages. 

MongoDB comparison operators allow you to get a subset of documents based on a comparison. For instance, instead of getting one document by its `_id` or all documents from a collection, you can get only the documents that have a field with a value to meets the condition of your comparison operator. For instance, you can retrieve only the documents that have a field named `score` with a value greater than `10`. Or you can get documents that have a field named `first_name` with a value of `'Jane'`. 

Using comparison operators in MongoDB is similar to but different from using comparison operators in other programming languages. The similarities are conceptual. For instance, greater than is greater than. Less than is less than. However, some key differences are syntax and order of operations. MongoDB uses syntax like `$gt` and `$lt`, whereas languages like JavaScript use mathematical symbols like `>` and `<`. Plus, you use comparison operators generally through your code in other languages, whereas in MongoDB they belong inside an object that serves as an argument inside a function call. 

Unfortunately, the order of operations in MongoDB is not exactly the same as for algebra. To understand the comparison order, visit [MongoDB's page about comparison/sort order](#https://docs.mongodb.com/manual/reference/bson-type-comparison-order/#bson-types-comparison-order).

Here is a list of MongoDB comparison operators:

`$eq`  Equal to: Matches values that are equal to a specified value.  
`$gt`  Greater than: Matches values that are greater than a specified value.  
`$gte`  Greater than or equal to: Matches values that are greater than or equal to a specified value.  
`$in`  In an arrayL Matches any of the values specified in an array.  
`$lt`  Less than: Matches values that are less than a specified value.  
`$lte`  Less than or equal to: Matches values that are less than or equal to a specified value.  
`$ne`  Not equal to: Matches all values that are not equal to a specified value.  
`$nin`  None: Matches none of the values specified in an array.  

Seeing some examples below will demonstrate.

## [Examples of comparison operators](#examples-of-comparison-operators)

## [List of logical operators](#list-of-logical-operators)

`$and`  Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.  
`$not`  Inverts the effect of a query expression and returns documents that do not match the query expression.  
`$nor`  Joins query clauses with a logical NOR returns all documents that fail to match both clauses.  
`$or`  Joins query clauses with a logical OR returns all documents that match the conditions of either clause.  

## [Examples of logical operators](#examples-of-logical-operators)

