# What is MongoDB?

MongoDB is a popular database platform for storing information for web applications. According to the [MongoDB website](https://www.mongodb.com/), MongoDB "is a general purpose, document-based, distributed database built for modern application developers and for the cloud era."

A database is an organized collection of digital data stored on a computer. In the physical world, a database is like a file cabinet or warehouse, and the digital information store within is like the folders and papers inside a file cabinet or the boxes and other things stored inside a warehouse. Whereas a file cabinet is located in a building and a warehouse in a neighborhood, a database is located on a computer.

Like a file cabinet or warehouse needs workers who know how to find and maintain the things stored within, a database needs software for managing. Not only does MongoDB store the data, it also is the software you need to interact with the data. Like with many databases, with MongoDB you can interact in powerful ways with information stored within. For instance, you can search, reference, compare, change and otherwise manipulate the stored information. You can do so with optimal speed and minimal processing expense.

You can work locally on your computer with MongoDB or you can use MongoDB's cloud-based services. Using the cloud services, you can access your database so long as you have internet access. In this lesson, you will focus on using the cloud-based service called [MongoDB Atlas](https://docs.atlas.mongodb.com/). From their website, "MongoDB Atlas is a fully-managed cloud database."

Unlike SQL databases where you store information in tables, MongoDB is known as NoSQL because it doesn't store information in tables. Rather, it stores information in documents. In MongoDB, each database can have multiple collections of documents. You can fill each collection with whatever information you want, but typically a collection is made up of documents containing the same or similar information. For instance, a collection for all your users. Or a collection for all the purchases made from your e-commerce store. Or a collection for all comments made on your blog. But probably not a collection that contains a mixture of users, purchases and comments.

The documents are JSON-like, which makes them flexible to work with. One of the main JSON-like features is that the structure of each document in a collection does not have to be identical to the structure of the other documents in that collection. For instance, in your users collection, some users may have a first name, last name, birthday, and user_id. However, not all users are required to have all fields. It's okay if some users are missing one or more of first name, last name, birthday, and user_id. This is different crom SQL, which requires each entry (the SQL-equivalent of a document) in a table (the SQL-equivalent of a collection) to have all the same fields as the other entries in that same table.

You can use MongoDB with many different programming languages, like NodeJS, Python, and others. In this lesson, you will focus on working with MongoBD and NodeJS. Although this lesson focuses on NodeJS, what you learn here will make it easy to learn how to work with MongoDB generally. 

 
## [Setting up MongoDB Atlas](#setting-up-mongoDB-atlas)

Getting started with MongoDB Atlas is easy. Visit the [MongoDB Create Account page](https://account.mongodb.com/account/register) to register your account. You can create a MongoDB account using your Google account or your email. Follow the registration instructions. At some point during the registration process, MongoDB will ask you to choos a path (or plan). Choose the free option. 

In addition, MongoDB will ask for you to choose a cloud provider, region, and other specifications. Choose whichever cloud provider you prefer. Choose M0 Sandbox option. Name your cluster whatever you want. For the remainder of this lesson, we will assume you named your cluster Project 0.

At the end of registration, MongoDB will prompt you to create a cluster. Do it. A cluster will be home to your databases. It'll take a few minutes for your cluster to build itself. Do not navigate away from the page yet.

## [Connect IP Address](#connect-ip-address)

Now that you have a MongDB account and a free-tier cluster, you should configure your MongoDB Atlas settings. First, you should connect your IP address to MongoDB Atlas. You will learn how below.

Each device connected to the internet has an IP address. An IP address is unique for each device. An IP address is used to identify a device on the Internet or a local network, like an address for a house. 

You can only connect to a MongoDB cluster if you are connecting from a device with an IP address that MongoDB Atlas trusts. Add your IP address to the list of trusted IP addresses in your MongoDB account. That way you can connect to your MongoDB account without worry that others can connect without your permission.

Find the IP address for your computer. If your device is connected to a network, it will have a public (external) and private (internal) IP address. The public IP address is the address for the Network. It's how the public will see your address. However, inside the network, your device has its own private IP address.

It's the public IP address that you'll need for MongoDB. To find your public (network) IP address, you can use the website aptly named [What is My IP Address](https://whatismyipaddress.com/).

In case you ever need to know your private IP address, the following should help. Multiple ways exist for finding your IP address. Find your Network settings. On MacOS, you can go to System Preferences, and inside that find the Network icon. On the Network page, you should see something like `Wi-Fi is connected to name-of-your-wifi and has the IP address XXX.XXX.X.XX.` On Windows, open the start menu and right-click the Network option. Choose the properties option and then choose View Status. Then, choose Details and look for the IP address in the new window.

After finding your public IP address, you should add it to your MongoDB IP access list so that you can connect to your MongoDB cluster. On the homepage for your Project 0 cluster, your Sandbox area. Inside the sandbox, you will see three buttons. One of those buttons should say Connect. Click it. 

A prompt should open with an option to Add a connection IP address. The box might be auto-filled with your public IP address. If it is, double check to make sure it's correct. If not, add your public IP address. Be sure to click the button for Add IP Address. 

In that same prompt, MongoDB will ask you to create a database user. This is important for managing and accessing your MongoBD Atlas databases. As the prompt says, remember your username and password because you will need it later. After you add the user, click the button in the prompt that says Choose a connection method.

The next page in the prompt will offer you multiple options for connecting to your MongoDB Atlas cluster. Choose the "connect your application" option. On the following page, choose NodeJS as the driver and the most recent version. Copy the connection string and then click the close button.

## [Connect to Cluster](#connect-to-cluster)

Connect your application to the MongoDB cluster you created. You have multiple options for connecting. You can connect using the MongoDB Drivers or the MongoDB Shell. This lesson shows you how to connect using the MongoDB Driver for NodeJS (using the [example from the MongoDB website](https://docs.atlas.mongodb.com/tutorial/connect-to-your-cluster/)).

To connect your application to MongoDB, create a project folder. Inside the project folder, add a file named connect.js. In the connect.js file, add the following code. In the line that defines the url (i.e., `const url = ...`), be sure to replace the string with the connection string you copied in the previous section when you connected your IP address to your MongoDB account. If you didn't copy that string, go back to the previous section and copy the string. 

```node
const { MongoClient } = require("mongodb");
 
// Replace the following with your Atlas connection string                                                                                                                                        
const url = "mongodb+srv://<username>:<password>@clustername.mongodb.net/test?retryWrites=true&w=majority&useNewUrlParser=true&useUnifiedTopology=true";
const client = new MongoClient(url);

async function run() {
    try {
        await client.connect();
        console.log("Connected correctly to server");

    } catch (err) {
        console.log(err.stack);
    }
    finally {
        await client.close();
    }
}

run().catch(console.dir);
```

If you try running your code now (entering in the terminal `node connect.js`), you'll get an error that looks something like this.

```node
internal/modules/cjs/loader.js:983
  throw err;
  ^

Error: Cannot find module 'mongodb'
Require stack:
- /Users/jonathangrossman/Documents/Developer/ITC/Work/mongodb/mongo_example/connect.js
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:980:15)
    at Function.Module._load (internal/modules/cjs/loader.js:862:27)
    at Module.require (internal/modules/cjs/loader.js:1042:19)
    at require (internal/modules/cjs/helpers.js:77:18)
    at Object.<anonymous> (/Users/jonathangrossman/Documents/Developer/ITC/Work/mongodb/mongo_example/connect.js:1:25)
    at Module._compile (internal/modules/cjs/loader.js:1156:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1176:10)
    at Module.load (internal/modules/cjs/loader.js:1000:32)
    at Function.Module._load (internal/modules/cjs/loader.js:899:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:74:12) {
  code: 'MODULE_NOT_FOUND',
  requireStack: [
    '/Users/jonathangrossman/Documents/Developer/ITC/Work/mongodb/mongo_example/connect.js'
  ]
}
```

Despite the intimidating look of this error message, the answer to the error is in there. The message includes `Error: Cannot find module 'mongodb'`. This tells you that you need to install the mongodb package. If you look in your root folder, you don't have a node_modules folder, which means you don't have any packages. 

Add the mongodb package to your project by entering the following in your terminal `npm install mongodb`. After entering the installation command, confirm that the message in the terminal says intallation succeeded. Also, look in your project's root folder to make sure a node_modules folder and package-json.lock file each exist.

Enter in the terminal `node connect.js`. If you successfully connected your application, the terminal should print `Connected correctly to server.`

Let's discuss the code above. The first line `const { MongoClient } = require("mongodb")`, imports the the MongoClient() class definition into your `connect.js` file. The next line `const url = . . .` defines the connection string for your MongoDB cluster. You need this connectrion string url because you are going to create an instance of the MongoClient() class, and to create an instance of that class, you need to pass it an argument. That argument needs to be the connection string.

The next line in your code `const client = new MongoClient(url);` creates an instance of the MongoClient() class using the connection string (url) as the argument. The variable `client` is now a MongoClient object customized with the connection string for your MongoDB Atlas cluster.

Following instantiation of the MongoClient() class is a block of asynchronous code with try / catch / finally blocks nested within. In the try block is a line that connects your application to MongoDB Atlas `await client.connect();`. Remember, `client` is your instance of the MongoDB class customized with your MongoDB connection string. That means `.connect()` is a method of the instance of that class. This method does what its name suggests -- it connects your project folder to your MongodDB Atlas cluster.

Inside the `catch` block, you print to the console any errors `console.log(err.stack)`, and then inside the `finally` block, you call the close the connection between your application and MongoDB Atlas `await client.close();`. Like the `.connect()` method of the MongoClient() class instance, the `.close()` method comes from the `client` instance of the MongoClient() class.

Finally, you run your application `run().catch(console.dir);`.

## [Explore MongoClient Class](#explore-mongoclient-class)

```node
unction getAllFuncs(toCheck) {
  var props = [];
  var obj = toCheck;
  do {
    props = props.concat(Object.getOwnPropertyNames(obj));
  } while ((obj = Object.getPrototypeOf(obj)));

  return props.sort().filter(function (e, i, arr) {
    if (e != arr[i + 1] && typeof toCheck[e] == "function") return true;
  });
}

console.log(getAllFuncs(client));
```

## [Add Data To Database](#add-data-to-database)
Part 6: Insert and View Data in Your Cluster.
