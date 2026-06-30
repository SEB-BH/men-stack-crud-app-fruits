<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Use Mongoose to Connect to MongoDB</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to connect to a MongoDB cloud database using the Mongoose JavaScript driver.

## Connect to MongoDB

Before diving into building the functional aspects of our web application, we'll need to establish a connection to a database. This connection will enable our application to store and retrieve data as we develop more features. To achieve this, we will use MongoDB Atlas, a cloud database service, along with Mongoose, an Object Data Modeling (ODM) library for MongoDB and Node.js. Additionally, we'll use the `dotenv` package to manage our environment variables, which includes securely storing our database connection string.

## Install `mongoose` and `dotenv` from NPM

To use mongoose in our application, we'll first need to install the `mongoose` and `dotenv` packages from NPM. Stop your server and install the following:

```bash
npm i mongoose dotenv
```

## Add the MongoDB Atlas connection string to a `.env` file

Next, create a `.env` file in your project's root directory:

```bash
touch .env
```

This file will be used to store any sensitive, secret information that the application needs to run, but that we don't want to commit to GitHub. Database connection strings definitely qualify: we don't want just anybody to access our database using this string! To ensure that our `.env` file doesn't make its way up to GitHub, create a `.gitignore` file:

```bash
touch .gitignore
```

In that file, add the following:

```plaintext
.env
node_modules
```

The first line above will make sure that the `.env` file isn't tracked by Git. The second line does the same thing, but for the `node_modules` directory that contains all the dependencies that `npm` has already installed or will install in the future. Ignoring `node_modules` is not about security, though. Rather, it reduces the amount of code in our repo by eliminating third party packages that can easily be installed via `npm`.

Now that we're certain Git won't track it, let's edit our `.env` file.

`.env` files are a simple list of key-value pairs. 

For example:

```plaintext
SECRET_NUMBER=13
PASSWORD=12345
```

The above code would allow an application to access the `SECRET_NUMBER`, and `PASSWORD` properties on a `process.env` object.

### Add connection string

You can now paste your MongoDB Atlas connection string into your app's `.env` file, assigning it to a `MONGODB_URI` environment variable. 

For example:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@sei.azure.mongodb.net/?retryWrites=true
```

> Do not use the above connection string in your application, it will not work. Paste the connection string obtained from your personal MongoDB Atlas account.

> Note: It is important that there are no spaces between `MONGODB_URI`, `=`, and your atlas connection string. It should be written as one continuous string with no spaces.

This will make the connection string available in our application on the `process.env.MONGODB_URI` property.

### Name your database collection

Your connection string will default to a generic unnamed database collection as indicated by the `/?` towards the end of the connection string. However, you **_must_** update this to your preferred collection name. In this application, that will be `fruits`. You can specify the preferred collection name by adding it between the `/` and the `?` in the connection string. 

Here's how that will look for this app: `/fruits?`. The full connection string for this app will read like this:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@sei.azure.mongodb.net/fruits?retryWrites=true
```

> Again, do not use the above connection string in your application, it will not work. Simply note where `/fruits?` exists in the value for `MONGODB_URI` and replicate it in your own version.

Anytime you need to make a new app, you can use this same connection string and only replace the `/fruits?` portion of the string. Ensure the name you assign is unique to that project - for example, once we've used the name `fruits` for this app, you shouldn't use it again. Also, your collection name should not contain any special characters.

## Connecting to MongoDB in `server.js`

With our environment variables added, we need to require the `dotenv` package in our `server.js` file to access them:

At the **_top_** of your `server.js` file, add:

```js
//server.js

const dotenv = require("dotenv"); // require package
dotenv.config(); // Loads the environment variables from .env file
const express = require("express");

const app = express();

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

It's important that these two lines of code are at the top of this file - it ensures that the environment variables are available everywhere across your application.

Next, we'll need to require `mongoose` so that we can use it to connect to our database:

```javascript
// server.js

const dotenv = require("dotenv");
dotenv.config();
const express = require("express");
const mongoose = require("mongoose"); // require package

const app = express();

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

Now we can use the `mongoose.connect()` method to connect to our database. We'll pass this method a single argument - the connection string from our `.env` file. This is the environment variable we just added in our `.env` file - `MONGODB_URI`. 

Add the following line after your require statements:

```javascript
// server.js

const dotenv = require("dotenv");
dotenv.config();
const express = require("express");
const mongoose = require("mongoose");

const app = express();

// Connect to MongoDB using the connection string in the .env file
mongoose.connect(process.env.MONGODB_URI);

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

To ease debugging, we'll also add a connection message that will print to our terminal when we've connected to the database. 

Add it below the `mongoose.connect()` method:

```javascript
// server.js

const app = express();

mongoose.connect(process.env.MONGODB_URI);
// log connection status to terminal on start
mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

This is a Mongoose event listener that runs the supplied callback function once we have connected to a database. In the callback function, we log the name of the database to which we've connected - in this case, it should be `fruits`.

## Start the app

Launch the application with `nodemon`. You should see two messages in your terminal - one confirming that the app is ready, and a second confirming that you are connected to your database.

> ♻️ Repeatable pattern: Any app you build that connects to a MongoDB database in Node using Mongoose will need a `.env` file with a connection string inside of it, and a `mongoose.connect()` method in your `server.js` file. You will pass this method the environment variable you created in your `.env` file - in this case `MONGODB_URI`.