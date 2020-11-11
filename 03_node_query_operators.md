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

## [Comparison](#comparison)

For comparison of different BSON type values, see the specified BSON comparison order.

`$eq`: Matches values that are equal to a specified value.  
`$gt`	      Matches values that are greater than a specified value.  
`$gte`	      Matches values that are greater than or equal to a specified value.  
`$in`	      Matches any of the values specified in an array.  
`$lt`	      Matches values that are less than a specified value.  
`$lte`	      Matches values that are less than or equal to a specified value.  
`$ne`	      Matches all values that are not equal to a specified value.  
`$nin`	      Matches none of the values specified in an array.  

## [Logical](#logical)

`$and`	      Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.  
`$not`	      Inverts the effect of a query expression and returns documents that do not match the query expression.  
`$nor`	      Joins query clauses with a logical NOR returns all documents that fail to match both clauses.  
`$or`	      Joins query clauses with a logical OR returns all documents that match the conditions of either clause.  
