<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Delete a Fruit</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to implement and utilize the `delete` route in an Express application to remove items from a MongoDB database.

## The `delete` route

The purpose of this route is to remove a specific fruit from the database. If we follow RESTful routing conventions, we know our route will be `DELETE /fruits/:fruitId`, with `fruitId` referring to the id of the fruit we intend to delete.

## Middleware

To further enhance our application, we'll introduce two essential middleware components: `method-override` and `morgan`, to assist us with our delete route. We'll begin with `method-override` here and cover `morgan` later. 

By default, web browsers only allow for the `method` attribute of a form to be set to either `POST` or `GET`. This is in direct conflict with our RESTful routing convention, which uses the HTTP methods `PUT` and `DELETE` for certain routes. The solution is to use the `method-override` middleware, which essentially tricks our express app into thinking that we've made `PUT` and `DELETE` requests from the browser. By doing this, we're able to stick to our routing conventions, while at the same time, behind the scenes, using HTTP methods that the browser supports. More details below.

The `morgan` middleware serves as a logging tool for our HTTP requests, providing valuable insights into application behavior. Again, we'll see it in action below.

In order to use both of these, we need to stop our server and install their node packages:

```bash
npm i method-override morgan
```

Next, let's require them at the top of our `server.js` file just below our other dependencies:

```js
// server.js
const app = express();
const mongoose = require('mongoose');
const methodOverride = require("method-override"); // new
const morgan = require("morgan"); //new

// DB connection code

// Mount it along with our other middleware, ABOVE the routes
app.use(express.urlencoded({ extended: false }));
app.use(methodOverride("_method")); // new
app.use(morgan("dev")); //new

// routes below
```

Restart your server and check the terminal to see morgan in action. 

## Create the UI to `delete` a fruit

Now we need to adjust our fruits `show` page to include a button for deleting a fruit.

Update `show.ejs` with the following:

```html
<h1><%= fruit.name %></h1>
<% if(fruit.isReadyToEat){ %>
<p>This fruit is ready to eat!</p>
<% } else { %>
<p>This fruit is not ready to eat!</p>
<% } %>
<!-- new delete button -->
<form action="/fruits/<%=fruit._id%>?_method=DELETE" method="POST">
  <button type="submit">Delete <%= fruit.name %></button>
</form>
<a href="/fruits">Back to Fruits</a>
```

All we've added here is a `<form>` with a single button in it. This button, when clicked, sends a request to the server to delete a particular fruit.

**Form Action and Method:**

- The action attribute in the form is set to the URL `/fruits/<%=fruit._id%>`, where `<%=fruit._id%>` dynamically inserts the `ID` of the current fruit. This tells the form where to send the request.

- Normally, forms only support `GET` or `POST` methods, but we want to send a `DELETE` request. To achieve this, we use a query parameter `?_method=DELETE` in the action URL. This tricks our server into treating the `POST` request as a `DELETE` request, thanks to the `method-override` middleware we set up in `server.js`.

The form doesn't have any input fields because we only need the fruit's `ID` to delete it, which is already included in the form's action URL.

## Define the route

To build and test the delete functionality in stages, we'll first create a basic route that sends a confirmation message. Then, we'll refactor it to add the actual delete functionality.

Add the following below your other routes in `server.js`:

```js
app.delete("/fruits/:fruitId", (req, res) => {
  res.send("This is the delete route");
});
```

This route uses `app.delete` to listen for delete requests. When a delete request is made to `/fruits/:fruitId`, it will respond with a message saying "This is the delete route". This step ensures that the delete route is being accessed correctly when the delete button is clicked.

Test a delete button in the browser. 


## Create `delete` functionality

For this route we'll use the Mongoose method `findByIdAndDelete()` to find the fruit by its `ID` and delete it. Just like the `show` route, fruit `ID` is obtained from the URL parameter `req.params.fruitId`.

Once you've confirmed the route works as expected, refactor it to include the actual delete functionality:

```js
app.delete("/fruits/:fruitId", async (req, res) => {
  await Fruit.findByIdAndDelete(req.params.fruitId);
  res.redirect("/fruits");
});
```

After deleting the fruit, we will redirects the user back to the `index` page `/fruits`, where the deleted fruit will no longer be listed.

Test this updated functionality by visiting the show page of any fruit and clicking the delete button. You should be redirected to the `index` page, where the deleted fruit should no longer appear.

## View `morgan` logs

If you check your terminal, you may notice additional information regarding the various requests has been logged. This is what the `morgan` middleware does for us. It's a nice way to have some debug information automatically logged for us without us manually setting it up. If at any point, you feel your terminal is getting crowded with additional information you don't need, simply comment out `app.use(morgan('dev'));`


If you are able to delete a fruit, you have successfully performed your next CRUD operation, **Delete**. Congrats!