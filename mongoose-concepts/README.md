<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Connect to MongoDB with Mongoose</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to connect an Express app to a MongoDB database using Mongoose, a connection string, and a `.env` file.

## Why do we need a database?

Right now, our Fruits app can show a landing page.

That is a great start, but our app cannot remember anything yet.

Soon, users will be able to add fruits, view fruits, update fruits, and delete fruits. For that to work, our app needs a place to store data.

That place is a **database**.

A database lets an app save information so the data is still there later, even after the server restarts.

For example, our app should be able to store fruit data like this:

```js
{
  name: "Mango",
  isReadyToEat: true
}
```

## What is MongoDB?

**MongoDB** is a database.

MongoDB has a few important parts:

| Part           | Meaning                                  |
| -------------- | ---------------------------------------- |
| **Database**   | The main container for one app's data    |
| **Collection** | A group of related documents             |
| **Document**   | One piece of data, similar to one object |


For our Fruits app:

| MongoDB part | Example                       |
| ------------ | ----------------------------- |
| Database     | `fruits_db`                   |
| Collection   | `fruits`                      |
| Document     | One fruit, like Mango or Date |

> 💡 The database and collection can sometimes have the same name. They are still different things.s

MongoDB stores data in documents that look similar to JavaScript objects.

A fruit document might look like this:

```js
{
  name: "Date",
  isReadyToEat: true
}
```

This feels familiar because we have already worked with JavaScript objects.

## What is Mongoose?

**Mongoose** is a JavaScript package that helps our Express app talk to MongoDB.

MongoDB is the database.

Mongoose is the tool we use in Node and Express to work with that database.

Mongoose will help us:

* Connect to MongoDB
* Create a structure for our fruit data
* Add new fruits
* Find fruits
* Update fruits
* Delete fruits

Later, we will create a **model** for our fruits. A model gives our data a clear structure.

For now, our goal is simple: connect the app to MongoDB.

## What is a connection string?

A **connection string** is a special URL that tells our app how to connect to MongoDB.

It usually includes:

* Your MongoDB username
* Your MongoDB password
* Your MongoDB cluster address
* The database name

It may look something like this:

```plaintext
mongodb+srv://<username>:<password>@cluster0.example.mongodb.net/fruits_db?retryWrites=true&w=majority
```

> 🚨 Do not copy the example above into your app. It will not work. You must use the connection string from your own MongoDB Atlas account.

Notice this part:

```plaintext
/fruits_db?
```

In this app, our database name will be `fruits_db`.

The database name goes after `.net/` and before `?`.

That tells MongoDB which database this app should use.

## Why do we use a `.env` file?

The connection string contains private information.

It may include your database username and password.

We do **not** want to push that information to GitHub.

Instead, we store private values in a `.env` file.

The `.env` file will stay on your computer and will not be uploaded to GitHub because we will add it to `.gitignore`.

