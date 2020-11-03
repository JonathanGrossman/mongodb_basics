# What is MongoDB?

MongoDB is a popular database platform for storing information for web applications. According to [its website](#https://www.mongodb.com/), MongoDB "is a general purpose, document-based, distributed database built for modern application developers and for the cloud era."

A database is an organized collection of digital data stored on a computer. In the physical world, a database is like a file cabinet or warehouse, and the digital information store within is like the folders and papers inside a file cabinet or the boxes and other things stored inside a warehouse. Whereas a file cabinet is located in a building and a warehouse in a neighborhood, a database is located on a computer.

Like a file cabinet or warehouse needs workers who know how to find and maintain the things stored within, a database needs software for managing. Not only does MongoDB store the data, it also is the software you need to interact with the data. Like with many databases, with MongoDB you can interact in powerful ways with information stored within. For instance, you can search, reference, compare, change and otherwise manipulate the stored information. You can do so with optimal speed and minimal processing expense.

You can work locally on your computer with MongoDB or you can use MongoDB's cloud-based services. Using the cloud services, you can access your database so long as you have internet access. In this lesson, you will focus on using the cloud-based service called [MongoDB Atlas](#https://docs.atlas.mongodb.com/). From their website, "MongoDB Atlas is a fully-managed cloud database."

Unlike SQL databases where you store information in tables, MongoDB is known as NoSQL because it doesn't store information in tables. Rather, it stores information in documents. In MongoDB, each database can have multiple collections of documents. You can fill each collection with whatever information you want, but typically a collection is made up of documents containing the same or similar information. For instance, a collection for all your users. Or a collection for all the purchases made from your e-commerce store. Or a collection for all comments made on your blog. But probably not a collection that contains a mixture of users, purchases and comments.

The documents are JSON-like, which makes them flexible to work with. One of the main JSON-like features is that the structure of each document in a collection does not have to be identical to the structure of the other documents in that collection. For instance, in your users collection, some users may have a first name, last name, birthday, and user_id. However, not all users are required to have all fields. It's okay if some users are missing one or more of first name, last name, birthday, and user_id. This is different crom SQL, which requires each entry (the SQL-equivalent of a document) in a table (the SQL-equivalent of a collection) to have all the same fields as the other entries in that same table.

You can use MongoDB with many different programming languages, like NodeJS, Python, and others. In this lesson, you will focus on working with MongoBD and NodeJS. Although this lesson focuses on NodeJS, what you learn here will make it easy to learn how to work with MongoDB generally. 

 
## [Setting up MongoDB Atlas](#setting-up-MongoDB-Atlas)

Getting started with MongoDB Atlas is easy. Visit the [MongoDB Create Account page](#https://account.mongodb.com/account/register) to register your account. You can create a MongoDB account using your Google account or your email. Follow the registration instructions. At some point during the registration process, MongoDB will ask you to choos a path (or plan). Choose the free option. 

In addition, MongoDB will ask for you to choose a cloud provider, region, and other specifications. Choose whichever cloud provider you prefer. Choose M0 Sandbox option. Name your cluster whatever you want. 

At the end of registration, MongoDB will prompt you to create a cluster. Do it. A cluster will be home to your databases. It'll take a few minutes for your cluster to build itself. 


https://docs.atlas.mongodb.com/getting-started/


## [Connect IP Addresses](#connect-IP-Addresses)

Now that you have a MongDB account and a free-tier cluster, you should configure your MongoDB Atlas settings. First, you should make sure you connect your IP address to MongoDB Atlas. Each device connected to the internet has an IP address. An IP address is unique for each device. An IP address is used to identify a device on the Internet or a local network, like an address for a house. 

You can only connect to a MongoDB cluster if you are connecting from a device with an IP address that MongoDB Atlas trusts. Add your IP address to the list of trusted IP addresses in your MongoDB account. That way you can connect to your MongoDB account without worry that others can connect without your permission.

Procedure
You must add your IP address to the IP access list before you can connect to your cluster. To add your IP address to the IP access list:

## [Create Database](#create-Database)
Part 4: Create a Database User for Your Cluster.

## [Connect to Cluster](#connect-to-Cluster)
Part 5: Connect to Your Cluster.

## [Add Data To Database](#add-Data-To-Database)
Part 6: Insert and View Data in Your Cluster.
