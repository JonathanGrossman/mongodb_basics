# MondoDB with NodeJS

At this stage of the process, you should have created a MongoDB Atlas account, created a cluster within your account, and added a database to your cluster. With this setup complete, you may now start interacting with your database. 

The main ways to interact with your database are using the CRUD operations. CRUD stands for create, read, update, and delete. To create is to put one or more new documents in your database. To read is to retrieve one or more documents from your database. To update is to edit one or more documents from your database. To delete is to delete one or more documents from your database. 

The syntax for the CRUD operations is fairly simple and is explained below. Despite the simplicity, a few tips and tricks will help you master these basics. Before discussing each CRUD operation, first you should have a better understanding of MongoDB documents.

https://docs.mongodb.com/manual/crud/

## [MongoDB Documents](#mongodb-documents)

Each entry in a MongoDB database is called a document. A document is a JSON-like object called a BSON. The BSON format generally looks and acts like JSON. For instance, like the JSON format has comma-separated key:value pairs wrapped in curly braces, the BSON format has comma-separated field:value pairs wrapped in curly braces. One difference, however, is that BSON objects can store more data types than JSON. For instance, in a MongoDB document, you can store other documents, arrays of document. Here is an example from the MongoDB website:

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

- _id holds an ObjectId.
- name holds an embedded document that contains the fields first and last.
- birth and death hold values of the Date type.
- contribs holds an array of strings.
- views holds a value of the NumberLong type.

https://docs.mongodb.com/manual/core/document/

## [Create](#create)

## [Read](#read)

## [Update](#update)

## [Delete](#delete)
