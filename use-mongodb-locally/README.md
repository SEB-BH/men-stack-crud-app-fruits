<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Use MongoDB Locally</span>
</h1>

## About

In this guide, we will connect our Express and Mongoose app to a MongoDB database running on our own computer.

Usually, we use **MongoDB Atlas**, which stores our database online. Sometimes, school, office, or public WiFi blocks the connection to Atlas. If that happens, we can use **local MongoDB** instead.

Local MongoDB means:

* The database runs on your computer.
* You do not need an Atlas IP access list.
* You do not need an internet connection after MongoDB is installed.
* Your app still uses Mongoose in almost the same way.

> 💡 This is a development setup. It is good for learning and building locally. For deployed apps, you will usually use a cloud database like MongoDB Atlas.

---

## What Will Change?

Only the **connection string** changes.

### Atlas connection string

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@cluster-name.mongodb.net/fruits?retryWrites=true
```

### Local connection string

```plaintext
MONGODB_URI=mongodb://127.0.0.1:27017/fruits
```

The rest of the app can stay the same.

You can still use:

```js
mongoose.connect(process.env.MONGODB_URI);
```

---

## Before You Start

You should already have:

* Node.js installed
* An Express app
* `mongoose` installed
* `dotenv` installed
* A `.env` file
* A `.gitignore` file that includes `.env`

If you need to install `mongoose` and `dotenv`, run this from your project folder:

```bash
npm i mongoose dotenv
```

---

## Step 1: Install MongoDB Locally

Choose the correct section for your computer.

---

## macOS Setup

Use these steps if you are on a Mac.

### Install MongoDB with Homebrew

Run these commands in Terminal:

```bash
brew tap mongodb/brew
brew update
brew install mongodb-community@8.0
```

### Start MongoDB

Run:

```bash
brew services start mongodb-community@8.0
```

### Check that MongoDB is running

Run:

```bash
brew services list
```

Look for `mongodb-community`.

You should see something like:

```plaintext
mongodb-community@8.0 started
```

### Optional: Stop MongoDB

You do not need to stop MongoDB during class, but this is the command:

```bash
brew services stop mongodb-community@8.0
```

---

## Windows Setup

Use these steps if you are on Windows.

### Install MongoDB Community Server

1. Go to the MongoDB Community Server download page.
2. Select:

   * **Version:** Current version
   * **Platform:** Windows
   * **Package:** MSI
3. Download the installer.
4. Open the `.msi` file.
5. Choose **Complete** setup.
6. Keep **Install MongoD as a Service** selected.
7. Keep the default service settings.
8. Install MongoDB Compass if the installer gives you the option.
9. Finish the installation.

> 💡 Installing MongoDB as a service means Windows can run MongoDB in the background.

### Check that MongoDB is running

Use the Windows search bar and search for:

```plaintext
Services
```

Open the Services app.

Find:

```plaintext
MongoDB
```

The status should say:

```plaintext
Running
```

If it is not running:

1. Right-click `MongoDB`.
2. Select **Start**.

---

## Step 2: Create or Update Your `.env` File

In the root of your project, create a `.env` file if you do not already have one:

```bash
touch .env
```

Add this line:

```plaintext
MONGODB_URI=mongodb://127.0.0.1:27017/fruits
```

> ⚠️ Use `127.0.0.1`, not `localhost`.
>
> `127.0.0.1` means “this computer.” It is usually more reliable with Node and Mongoose.

---

## Step 3: Check Your `.gitignore`

Your `.env` file should not be pushed to GitHub.

Create a `.gitignore` file if you do not already have one:

```bash
touch .gitignore
```

Add this:

```plaintext
.env
node_modules
```

---

## Step 4: Connect with Mongoose

At the top of `server.js`, require and configure `dotenv`:

```js
const dotenv = require("dotenv");
dotenv.config();

const express = require("express");
const mongoose = require("mongoose");

const app = express();
```

Then connect to MongoDB:

```js
mongoose.connect(process.env.MONGODB_URI);
```

Add a connection message:

```js
mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});
```

Your `server.js` may look similar to this:

```js
const dotenv = require("dotenv");
dotenv.config();

const express = require("express");
const mongoose = require("mongoose");

const app = express();

mongoose.connect(process.env.MONGODB_URI);

mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

---

## Step 5: Start Your App

Run your app:

```bash
nodemon
```

or:

```bash
node server.js
```

You should see:

```plaintext
Listening on port 3000
Connected to MongoDB fruits.
```

If you see `Connected to MongoDB fruits`, your app is connected to your local database.

---

## Step 6: View Your Data in MongoDB Compass

MongoDB Compass is a visual app for looking at your MongoDB databases.

Open MongoDB Compass.

Use this connection string:

```plaintext
mongodb://127.0.0.1:27017
```

Click **Connect**.

After your app creates data, you should see a database named:

```plaintext
fruits
```

Inside that database, you should see your collections.

---

## Using a Different Database Name

The last part of the connection string is the database name.

For this app, we use:

```plaintext
mongodb://127.0.0.1:27017/fruits
```

For a cookbook app, you might use:

```plaintext
mongodb://127.0.0.1:27017/cookbook
```

For a real estate app, you might use:

```plaintext
mongodb://127.0.0.1:27017/openhouse
```

> 💡 Use a different database name for each project. This keeps your data organized.

---

## Common Errors

### Error: `MongooseServerSelectionError`

This usually means Mongoose cannot reach MongoDB.

Check these things:

1. Is MongoDB installed?
2. Is MongoDB running?
3. Does your `.env` file have the correct connection string?
4. Did you restart your server after changing `.env`?

---

### Error: `ECONNREFUSED 127.0.0.1:27017`

This usually means MongoDB is not running.

#### On macOS

Run:

```bash
brew services start mongodb-community@8.0
```

Then restart your app.

#### On Windows

Open the **Services** app.

Find **MongoDB**.

Start the service.

Then restart your app.

---

### Error: The app starts, but I do not see my data

Check the database name in your `.env` file.

For example:

```plaintext
MONGODB_URI=mongodb://127.0.0.1:27017/fruits
```

This creates or uses a database named `fruits`.

If your database name is different, your data may be in another database.

---

### Error: I changed `.env`, but nothing changed

Restart your server.

Environment variables load when the server starts. If you change `.env`, you must stop and restart the server.

Use:

```bash
control + c
```

Then run:

```bash
nodemon
```

---

## Repeatable Pattern

Any local MongoDB app using Mongoose needs:

1. MongoDB running on your computer.
2. A `.env` file with a local connection string.
3. `dotenv.config()` near the top of `server.js`.
4. `mongoose.connect(process.env.MONGODB_URI)`.
5. A database name at the end of the connection string.

Example:

```plaintext
MONGODB_URI=mongodb://127.0.0.1:27017/fruits
```

Example connection:

```js
mongoose.connect(process.env.MONGODB_URI);
```

---

## Final Check

Before moving on, confirm all of these are true:

* MongoDB is installed.
* MongoDB is running.
* `.env` exists.
* `.env` contains `MONGODB_URI=mongodb://127.0.0.1:27017/fruits`.
* `.gitignore` includes `.env`.
* `server.js` uses `dotenv.config()`.
* `server.js` uses `mongoose.connect(process.env.MONGODB_URI)`.
* The terminal says `Connected to MongoDB fruits`.

Once all of these are true, your app is using local MongoDB successfully.
