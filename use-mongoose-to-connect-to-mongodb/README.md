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
To ensure that our `.env` file doesn't make its way up to GitHub, create a `.gitignore` file:

```bash
touch .gitignore
```

In that file, add the following:

```plaintext
.env
node_modules
```

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


### Name your database collection

Here's how our database name will look for this app: `/fruits_db?`. The full connection string for this app will read like this:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@sei.azure.mongodb.net/fruits_db?retryWrites=true
```

> Again, do not use the above connection string in your application, it will not work. Simply note where `/fruits_db?` exists in the value for `MONGODB_URI` and replicate it in your own version.

Anytime you need to make a new app, you can use this same connection string and only replace the `/fruits_db?` portion of the string. Ensure the name you assign is unique to that project - for example, once we've used the name `fruits_db` for this app, you shouldn't use it again. Also, your database name should not contain any special characters except for `_` as MongoDB prefers snake_case naming conventions.


## Load environment variables in `server.js`

Now we need to tell our app to read the `.env` file.

At the very top of `server.js`, add `dotenv`:

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

The `dotenv.config()` line loads the values from `.env`.

After this line runs, our app can use:

```js
process.env.MONGODB_URI
```

That gives us access to the connection string without writing it directly in `server.js`.

It's important that these two lines of code are at the top of this file - it ensures that the environment variables are available everywhere across your application.

## Connecting to MongoDB in `server.js`

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
  console.log(`Connected to MongoDB ${mongoose.connection.name} 🥭`);
});

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

This is a Mongoose event listener that runs the supplied callback function once we have connected to a database. In the callback function, we log the name of the database to which we've connected - in this case, it should be `fruits_db`.

## Start the app

Launch the application with `nodemon`. You should see two messages in your terminal - one confirming that the app is ready, and a second confirming that you are connected to your database.

## The repeatable pattern

Most Express apps that use MongoDB will follow this pattern:

1. Install `mongoose` and `dotenv`.
2. Store the connection string in `.env`.
3. Add `.env` to `.gitignore`.
4. Load `dotenv` in `server.js`.
5. Import `mongoose`.
6. Connect with `mongoose.connect(process.env.MONGODB_URI)`.

This is a pattern you will use many times as a developer.