# Using MongoDB in an Express Application

In the previous chapters, you learned the MongoDB basics. Although you worked with MongoDB code, you haven't yet connected that code to a server. In this chapter, you will build an express server and connect it to MongoDB. You also will write routes and work with the data from MongoDB inside your express app. 

This chapter is different than previous ones. Previous chapters were instructional and demonstrative, whereas this chapter is more of a guided building session. In this building session, you will make an Express application that connects to MongoDB. The Express application will have routes that allow you to manages users of your application. Your routes will get all users, get a single user, and add a user.

## Step 1. Setup Express

1. What imports do you need for Express?
2. How do you create an instance of the Express class?
3. Do you know now of other imports you will need to start your Express server?
4. How do you get your Express server to run and stay active?

## Step 2. Connect Express to MongoDB

1. What are the steps for connecting your express app to MongoDB Atlas?

- Do you need to install or import any software?
- Do you have a MongoDB connection string?
- What are you naming your database?
- What are you naming your collection?

2. How can you confirm that the connection is successful?

- Do you need to?
- What are the pros and cons?

## Step 3. Get all users

1. Write the boilerplate for the route

- Which http method should you use?
- For whichever http method you use, how many arguments does that express method accept?
- What are the data types those arguments?
- How will you handle errors inside your route function?

2. Write your MongoDB request

- Which MongoDB method should you use to get all users?
- How do you write the line of code to use that method?
- Upon receiving a response from MongoDB, do you need to process the response before serving it to the client?
- How do you serve the response to the client?
- Do you need to install any extra packages?

## Step 4. Get one user

1. Write the boilerplate for the route

- Which http method should you use?
- For whichever http method you use, how many arguments does that express method accept?
- What are the data types those arguments?
- How will you handle errors inside your route function?
- When you send the request from the client, what information should you send to tell the server which user you want?
- How will you send the information from the client to the server?
- Inside your route, how will you access the information that identifies the user requested by the client?

2. Write your MongoDB request

- Which MongoDB method should you use to get a single user?
- How do you write the line of code to use that method?
- What data type do you pass into the MongoDB method as an argument?
- Upon receiving a response from MongoDB, do you need to process the response before serving it to the client?
- How do you serve the response to the client?
- Do you need to install any extra packages?

## Step 5. Add a user

1. Write the boilerplate for the route

- Which http method should you use?
- For whichever http method you use, how many arguments does that express method accept?
- What are the data types those arguments?
- How will you handle errors inside your route function?
- What information do you need to send from the client to the server?
- How will you send that information?
- Inside your route, how will you get the information sent from the client?

2. Write your MongoDB request

- Which MongoDB method should you use to add a new user?
- How do you write the line of code to use that method?
- What data type do you pass into the MongoDB method as an argument?
- How will you know if the user was successfully added to the MongoDB database?
- How will you let the client know whether the user was successfully added?
- Do you need to install any extra packages?

## Step 6. Think about the possibilities

1. Upon receiving a response from MongoDB in your Express application for a **get** request, what information might you want to use to request more information from MongoDB (before even responding to the client)?
2. Upon receiving a response from MongoDB in your Express application for a **post** request, what information might you want to use to request more information from MongoDB (before even responding to the client)?
3. When storing information in a database, how will you organize your information?
4. If in addition to a user collection, you also have a blog collection containing blog posts by each user, how will you know which user is the author for each blog post?
5. How much search filtering should you do using MongoDB tools versus in your Express server script?
