<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Build the Fruit Model</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to define and export a Mongoose model.

## Creating a Model

As we progress with our application, the next essential step is to create a schema and model for our fruits. This process will define how our fruit data is structured and stored in the database. By establishing a clear schema, we ensure consistency and reliability in the data we handle. Additionally, the model created from this schema will serve as the main interface for our application to interact with the MongoDB database, allowing us to perform CRUD operations on fruit data. Let's dive into creating our first Mongoose schema and model for the fruits in our application.

## Create the model file `fruit.js`

Let's create a directory that will hold the file for our fruit model `fruit.js`:

```bash
mkdir models
touch models/fruit.js
```

Creating a dedicated directory for a single file may seem unnecessary, but it's actually a strategic approach in software development. This practice is beneficial for future scalability. For instance, if you later decide to add more models to your application, having a designated `models` directory allows for clear organization, with each model having its own separate file. This makes your codebase more structured and manageable.

Here's how we'll structure our `fruit.js` file:

- **Create the schema:** We'll begin by defining the schema for a `fruit`. This schema is like a blueprint, describing the properties and characteristics of what a `fruit` object should include.

- **Link the schema to a Model:** Next, we'll create a model from our schema. This model is essentially a representation of a MongoDB collection. By linking our schema to this model, we connect the defined structure of our `fruit` data to the corresponding collection in the database.

- **Export the Model:** Finally, we need to make sure our model is available for use elsewhere in our application. We'll do this by exporting the model from the fruit.js file.

You should name models and model files singularly. For example, `fruit.js` instead of `fruits.js`. This is because a model is a singular representation of the data you are storing.

## Create the fruits schema

Before we define our model, we must first import the mongoose library into our `fruit.js` file:

```javascript
// models/fruit.js

const mongoose = require("mongoose");
```

Now let's define the schema for our `fruit` model. A schema in Mongoose is a way to structure the data in our database. It's essentially a JavaScript object where each key represents a property of our data. The value assigned to each key specifies the type of data we expect for that property and can also include certain constraints or rules.

For our `fruit` model, we want to keep it simple. We'll have two properties: `name`, which will be a string, and `isReadyToEat`, which will be a boolean indicating whether the fruit is ready to be eaten. 

Here's what this would look like in our `fruit.js` file:

```javascript
// models/fruit.js

const mongoose = require('mongoose');

const fruitSchema = new mongoose.Schema({
  name: String,
  isReadyToEat: Boolean,
});
```

## Register the model

After defining our schema, the next step is to inform Mongoose about the collection in MongoDB that will use this schema for its documents. We achieve this by creating a model. A model in Mongoose serves as a constructor for creating new documents, and it also enforces the structure defined in the schema.

To create a model, we use the `mongoose.model` method. This method takes two arguments: the `name` of the model and the `schema` to apply to that model. 

Here's how we define the model for our `fruit` schema:

```javascript
// models/fruit.js

const mongoose = require('mongoose');

const fruitSchema = new mongoose.Schema({
  name: String,
  isReadyToEat: Boolean,
});

const Fruit = mongoose.model("Fruit", fruitSchema); // create model
```

> Note: There is a convention to use a capital letter for database model names, so name your model `Fruit`, as opposed to `fruit`.

## Export the model from the `fruit.js` file

Next, we want to export the `Fruit` model we just created, so that the rest of our application has access to it.

The `Fruit` model is what we will use to perform CRUD on the collection.

```javascript
// models/fruit.js

module.exports = Fruit;
```

## Import the model into `server.js`

Let's integrate the `Fruit` model into our `server.js`. This is important because the routes we define in `server.js` will need to use this model to interact with our MongoDB database. To do this, we'll add a require statement for our model in `server.js`. It's crucial to ensure that this require happens after we establish a connection to our MongoDB instance. This way, our model can effectively interact with the database once it's connected.

Here's how we can include the `Fruit` model in `server.js`:

```javascript
// server.js

mongoose.connect(process.env.MONGO_URI);

mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});

// Import the Fruit model
const Fruit = require("./models/fruit.js");
```

With this addition, we're ready to use our `Fruit` model in the request handling functions defined in our express routes. This setup will allow us to perform database operations like creating, reading, updating, and deleting fruit documents in MongoDB.