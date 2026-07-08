<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Practicing CRUD with One Route</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to use Mongoose to create, read, update, and delete fruit documents in MongoDB.

## Why are we using one route?

In a real Express application, each CRUD action should usually have its own route.

For example:

| Action   | HTTP Method | Route              |
| -------- | ----------- | ------------------ |
| Create   | `POST`      | `/fruits`          |
| Read all | `GET`       | `/fruits`          |
| Read one | `GET`       | `/fruits/:fruitId` |
| Update   | `PUT`       | `/fruits/:fruitId` |
| Delete   | `DELETE`    | `/fruits/:fruitId` |

But right now, we are still learning how Mongoose queries work.

So we will temporarily use one route as a **database playground**.

Add this route to `server.js`:

```js
app.get("/fruits", async (req, res) => {
  // We will keep changing this code.
});
```

This is not the final structure of our application.

We are doing this so we can focus on one new skill at a time: talking to MongoDB with Mongoose.

> 💡 For now, think of this route as a practice area. Later, we will move each query into the correct RESTful route.


Before we can create, find, update, or delete fruits, we need a **model**.

We have already imported our `Fruit` model into our `server.js` file.

This model says that each fruit should have:

| Field          | Type      | Example   |
| -------------- | --------- | --------- |
| `name`         | `String`  | `"Apple"` |
| `isReadyToEat` | `Boolean` | `true`    |

Now we are ready to practice CRUD.

## Part 1: Create a fruit

**Create** means add a new document to the database.

Update our `/fruits` route with the following:

```js
app.get("/fruits", async (req, res) => {
  const fruitData = {};

  fruitData.name = "Apple";
  fruitData.isReadyToEat = true;

  await Fruit.create(fruitData);

  res.send("I made a new fruit.");
});
```

Start your server:

```bash
nodemon
```

Visit this route in your browser:

```plaintext
http://localhost:3000/fruits
```

You should see this message:

```plaintext
I made a new fruit.
```

This means the route ran successfully.

But we should also check MongoDB.

## Check MongoDB Atlas

Go to MongoDB Atlas.

Open your database.

Look for:

```plaintext
Database: fruits_db
Collection: fruits
```

You should see a new document that looks similar to this:

```js
{
  _id: ObjectId('6a4e80051c3a032571e4ae92'),
  name: "Apple",
  isReadyToEat: true,
  __v: 0
}
```

MongoDB automatically creates the `_id`.

Mongoose automatically adds `__v`.

Do not worry about `__v` right now.

The important part is this:

```js
name: "Apple",
isReadyToEat: true
```

> 🚨 Every time you refresh `/fruits`, this route creates another Apple. That is why we are only using this as a temporary practice route.


## Part 2: Send the created fruit back to the browser

Right now, the browser only says:

```plaintext
I made a new fruit.
```

That works, but it does not show us the actual fruit that was created.

Update the route:

```js
app.get("/fruits", async (req, res) => {
  const fruitData = {};

  fruitData.name = "Apple";
  fruitData.isReadyToEat = true;

  const createdFruit = await Fruit.create(fruitData);

  res.send(createdFruit);
});
```

Refresh:

```plaintext
http://localhost:3000/fruits
```

Now the browser should show the fruit document.

It may look similar to this:

```js
{
  "name": "Apple",
  "isReadyToEat": true,
  "_id": "6a4e80051c3a032571e4ae92",
  "__v": 0
}
```

This line creates the fruit:

```js
const createdFruit = await Fruit.create(fruitData);
```

This line sends the created fruit back to the browser:

```js
res.send(createdFruit);
```

> 💡 This is useful while learning because we can immediately see what MongoDB created.

**Be sure to create a few more fruits before moving on - at least two should be `Mango`!**

## Part 3: Read all fruits

**Read** means get data from the database.

Now let’s stop creating new fruits and find all fruits instead.

Replace the route with this:

```js
app.get("/fruits", async (req, res) => {
  const allFruits = await Fruit.find({});

  res.send(allFruits);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

Now the browser should show an array of fruits.

It may look similar to this:

```js
[
  {
    "_id": "6a4e80051c3a032571e4ae92",
    "name": "Apple",
    "isReadyToEat": true,
    "__v": 0
  }
]
```

This line finds all fruit documents:

```js
const allFruits = await Fruit.find({});
```

The `{}` means:

```plaintext
Find everything.
```

So this query means:

```plaintext
Find all fruits.
```

## Part 4: Read fruits that match a condition

We can also find fruits that match a specific condition.

Replace the route with this:

```js
app.get("/fruits", async (req, res) => {
  const readyFruits = await Fruit.find({ isReadyToEat: true });

  res.send(readyFruits);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

Now the browser should only show fruits where `isReadyToEat` is `true`.

This line is the query:

```js
const readyFruits = await Fruit.find({ isReadyToEat: true });
```

This means:

```plaintext
Find all fruits where isReadyToEat is true.
```

Now try changing `true` to `false`:

```js
const notReadyFruits = await Fruit.find({ isReadyToEat: false });
```

Then update the `res.send()`:

```js
res.send(notReadyFruits);
```

The full route should look like this:

```js
app.get("/fruits", async (req, res) => {
  const notReadyFruits = await Fruit.find({ isReadyToEat: false });

  res.send(notReadyFruits);
});
```

If you do not have any fruits where `isReadyToEat` is `false`, you may see an empty array:

```js
[]
```

That is okay.

An empty array means:

```plaintext
The query worked, but it did not find matching documents.
```

## Part 5: Read one fruit by name

Now we can search for one fruit.

Replace the route with this:

```js
app.get("/fruits", async (req, res) => {
  const mango = await Fruit.findOne({ name: "Mango" });

  res.send(mango);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

This query searches for one fruit where the name is `"Mango"`:

```js
const mango = await Fruit.findOne({ name: "Mango" });
```

If Mongoose finds a Mango, it sends back one document.

If Mongoose does not find a Mango, it sends back:

```js
null
```

`null` means:

```plaintext
Nothing was found.
```


## Part 6: Update one fruit

**Update** means change a document that already exists.

Let’s update Mango and make it ready to eat.

Replace the route with this:

```js
app.get("/fruits", async (req, res) => {
  const updatedFruit = await Fruit.findOneAndUpdate(
    { name: "Mango" },
    { isReadyToEat: true },
    { new: true }
  );

  res.send(updatedFruit);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

This part finds the fruit:

```js
{ name: "Mango" }
```

This part updates the fruit:

```js
{ isReadyToEat: true }
```

This part tells Mongoose to send back the updated version:

```js
{ new: true }
```

Without `{ new: true }`, Mongoose may send back the old version before the update.

After you visit the route, check MongoDB Atlas.

Mango should now have:

```js
isReadyToEat: true
```


## Part 7: Update one fruit by `_id`

In real apps, we usually update by `_id`.

The `_id` is unique.

Two fruits could have the same name, but they will not have the same `_id`.

Go to MongoDB Atlas and copy the `_id` of one fruit.

Then replace `"PASTE_FRUIT_ID_HERE"` with your real fruit `_id`:

```js
app.get("/fruits", async (req, res) => {
  const updatedFruit = await Fruit.findByIdAndUpdate(
    "6a4e80051c3a032571e4ae92",
    { name: "Green Apple" },
    { new: true }
  );

  res.send(updatedFruit);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

Now check MongoDB Atlas again.

The fruit name should be updated to:

```plaintext
Green Apple
```

This query is more specific because it uses the fruit's unique `_id`.

## Part 8: Delete one fruit

**Delete** means remove a document from the database.

Go to MongoDB Atlas and copy the `_id` of the fruit you want to delete.

Then replace `"PASTE_FRUIT_ID_HERE"` with your real fruit `_id`:

```js
app.get("/fruits", async (req, res) => {
  const deletedFruit = await Fruit.findByIdAndDelete("6a4e80051c3a032571e4ae92");

  res.send(deletedFruit);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

The browser should show the fruit that was deleted.

Now check MongoDB Atlas.

That fruit should no longer be in the collection.

> 🚨 Be careful with delete queries. Once a document is deleted, it is gone from the collection.


## Part 9: Delete many fruits carefully

Sometimes we may want to delete many documents.

For example, we may want to delete all fruits named `"Apple"`.

Replace the route with this:

```js
app.get("/fruits", async (req, res) => {
  const result = await Fruit.deleteMany({ name: "Apple" });

  res.send(result);
});
```

Visit:

```plaintext
http://localhost:3000/fruits
```

The browser may show something like this:

```js
{
  "acknowledged": true,
  "deletedCount": 2
}
```

This means MongoDB deleted 2 matching documents.

This part is the filter:

```js
{ name: "Apple" }
```

It means:

```plaintext
Delete fruits where the name is Apple.
```

> 🚨 Never write `deleteMany({})` unless you truly want to delete everything in the collection.

This would delete all fruits:

```js
await Fruit.deleteMany({});
```

**Do not run that!**


## CRUD summary

We practiced CRUD using Mongoose.

| CRUD action | Meaning               | Mongoose method             |
| ----------- | --------------------- | --------------------------- |
| Create      | Add a new document    | `Fruit.create()`            |
| Read        | Find documents        | `Fruit.find()`              |
| Read one    | Find one document     | `Fruit.findOne()`           |
| Update      | Change a document     | `Fruit.findByIdAndUpdate()` |
| Delete      | Remove a document     | `Fruit.findByIdAndDelete()` |
| Delete many | Remove many documents | `Fruit.deleteMany()`        |


## Why did we use `app.get()` for everything?

We used `app.get("/fruits")` for every example because it was easy to test in the browser.

The browser can visit a `GET` route directly.

That made it simple for us to practice Mongoose queries.

But this is not how we should build the final app.

## Why should we not use `GET` to create, update, or delete?

A `GET` request should be used to read data.

A `GET` request should not change the database.

This route is a problem:

```js
app.get("/fruits", async (req, res) => {
  await Fruit.create({
    name: "Apple",
    isReadyToEat: true,
  });

  res.send("I made a new fruit.");
});
```

Why?

Because simply visiting this page creates data.

That means:

* Refreshing the page creates another fruit.
* Sharing the URL with someone else could create another fruit.
* The browser may request the page again.
* The route does something unexpected.

That is dangerous.

The route says:

```plaintext
GET /fruits
```

But it really means:

```plaintext
Create a fruit.
```

That mismatch makes the app harder to understand and easier to break.

## The correct pattern

Later, our Fruits app will use routes like this:

| User action            | HTTP method | Route                   | Purpose              |
| ---------------------- | ----------- | ----------------------- | -------------------- |
| View all fruits        | `GET`       | `/fruits`               | Read all fruits      |
| View one fruit         | `GET`       | `/fruits/:fruitId`      | Read one fruit       |
| Show new fruit form    | `GET`       | `/fruits/new`           | Display a form       |
| Submit new fruit form  | `POST`      | `/fruits`               | Create a fruit       |
| Show edit fruit form   | `GET`       | `/fruits/:fruitId/edit` | Display an edit form |
| Submit edit fruit form | `PUT`       | `/fruits/:fruitId`      | Update a fruit       |
| Delete a fruit         | `DELETE`    | `/fruits/:fruitId`      | Delete a fruit       |

For now, the important idea is:

> 💡 We used one `GET` route to learn Mongoose queries. In the real app, we will use the correct HTTP method for each action.

## 🎓 You Do

Use the same `app.get("/fruits")` route to practice.

Complete each task one at a time.

After each task, check the browser and MongoDB Atlas.

### Task 1

Create a new fruit named `"Banana"`.

The fruit should not be ready to eat.

Expected data:

```js
{
  name: "Banana",
  isReadyToEat: false
}
```

### Task 2

Find all fruits.

Send them to the browser.

### Task 3

Find only fruits where `isReadyToEat` is `true`.

Send them to the browser.

### Task 4

Find one fruit named `"Date"`.

Send it to the browser.

### Task 5

Update one fruit by `_id`.

Change its `isReadyToEat` value.

Send the updated fruit to the browser.

### Task 6

Delete one fruit by `_id`.

Send the deleted fruit to the browser.

## Final reminder

This route was only for practice:

```js
app.get("/fruits", async (req, res) => {
  // temporary Mongoose practice
});
```

After this lesson, we will stop using one route for everything.

Next, we will build proper RESTful routes for our full Fruits app.
