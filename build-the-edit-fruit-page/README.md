<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Build the Edit Fruit Page</span>
</h1>

**Learning objective:** By the end of this lesson, students will understand how to build and utilize the `edit` route in an Express application, enabling users to modify existing data in a database.

## The `edit` route

The `Edit` route presents the user with a form for editing the details of a single fruit in our database. If we follow RESTful routing conventions, the corresponding URL for this route is `GET /fruits/:fruitId/edit`, where `:fruitId` represents the unique identifier of the fruit we intend to edit. 

One thing worth noting is that this is the only RESTful route that contains three segments `/fruits/:fruitId/edit`

Also, compare your new/create and edit/update RESTful routes conventions. The full process of updating a fruit is similar to the process of creating a fruit in that both processes require two routes: a form route for collecting user data, and a second route for processing the data. For updating a fruit, the form route is called Edit and the processing route is called Update. 

Here's how the two processes compare:

| Process        | Form Route                         | Processing Route                |
| -------------- | ---------------------------------- | ------------------------------- |
| Create a fruit | New (`GET /fruits/new`)            | Create (`POST /fruits`)         |
| Update a fruit | Edit (`GET /fruits/:fruitId/edit`) | Update (`PUT /fruits/:fruitId`) |

In this section, we'll focus on the `edit` route. 

## Link to the edit page

To provide a way for users to navigate to the edit page of a specific fruit, we'll add a link on the `show` page. This link will be placed right after the delete button and before the link that takes the user back to the main fruit list. 

Update `show.ejs` with the following:

```html
<form action="/fruits/<%=fruit._id%>?_method=DELETE" method="POST">
  <button type="submit">Delete <%= fruit.name %></button>
</form>
<!-- add edit page link here -->
<a href="/fruits/<%= fruit._id %>/edit">Edit <%= fruit.name %></a>
<a href="/fruits">Back to Fruits</a>
```

In this code, we're dynamically generating the href attribute of the "Edit" link by inserting the unique `_id` of the fruit. This ensures that the link leads to the correct edit page for each specific fruit.

## Define the route

Because this route URL has three parts, it does not conflict with others and can be placed anywhere among our other routes. 

Add the following below the other routes in `server.js`:

```js
// GET localhost:3000/fruits/:fruitId/edit
app.get("/fruits/:fruitId/edit", async (req, res) => {
  const foundFruit = await Fruit.findById(req.params.fruitId);
  console.log(foundFruit);
  res.send(`This is the edit route for ${foundFruit.name}`);
});
```

Like our show route, we'll use `Fruit.findById(req.params.fruitId)` to find the specific fruit in our database. The `req.params.fruitId` captures the `ID` from the URL.

Since finding a fruit is an asynchronous operation, we use `async` before our route callback function and `await` before the `findById` method. This ensures that the server waits for the fruit to be found before moving to the next line of code.

Test the route by clicking the "Edit" link on the show page of a particular fruit. Check your terminal to make sure you see the data for the fruit that you intend to edit.

Lastly, let's modify our route so that it renders `views/fruits/edit.ejs`, passing in the `foundFruit` variable we created above:

```javascript
app.get("/fruits/:fruitId/edit", async (req, res) => {
  const foundFruit = await Fruit.findById(req.params.fruitId);
  res.render("fruits/edit.ejs", {
    fruit: foundFruit,
  });
});
```

Test the route to see an error- time to create the edit page!

## Create the `edit` template

Now that the route is set up, let's create the template. Create a `edit.ejs` template inside the `views/fruits`:

```bash
touch views/fruits/edit.ejs
```

For our Edit page, we'll need a form for editing fruit data. We can borrow the same form from our `new.ejs` page and make some adjustments:

```html
<body>
  <!-- update the header -->
  <h1>Edit <%= fruit.name %></h1>
  <form action="/fruits" method="POST">
    <label for="name">Name:</label>
    <input type="text" name="name" id="name" />

    <label for="isReadyToEat">Ready to Eat?</label>
    <input type="checkbox" name="isReadyToEat" id="isReadyToEat" />
    <!-- update the button text -->
    <button type="submit">Update Fruit</button>
  </form>
  <a href="/fruits">Back to Fruits</a>
</body>
```

Now let's edit the `<form>` tag:

```html
<form action="/fruits/<%=fruit._id%>?_method=PUT" method="POST"></form>
```

The Edit form needs to make a `PUT` request to `/fruits/:fruitID` in the same way that the New form needs to make a `POST` request to `/fruits`. 

Remember, that browsers don't support `PUT` requests, so we will use the same `method-override` middleware as we did with the delete route. We'll append `?_method=PUT` to the end of our action URL:

```html
<!-- updated form -->

<form action="/fruits/<%=fruit._id%>?_method=PUT" method="POST">
  <label for="name">Name:</label>
  <input type="text" name="name" id="name" />

  <label for="isReadyToEat">Ready to Eat?</label>
  <input type="checkbox" name="isReadyToEat" id="isReadyToEat" />

  <button type="submit">Update Fruit</button>
</form>
```


## Prefill form data

To improve user experience in our fruit editing form, we'll prefill the form with the current data of the fruit being edited. This makes it easier for users to see and modify the existing information. 

Here's how we can achieve this for both text input and checkboxes:

For the 'name' input field, we'll use the `value` attribute to prepopulate it with the fruit's name:

```html
<input type="text" name="name" value="<%= fruit.name %>" />
```

When the edit page loads, this input field will automatically display the name of the fruit being edited.

Checkboxes work differently. The checkbox for `isReadyToEat` will be checked based on whether the fruit is ready to eat or not. A checkbox input can have a `checked` attribute, which, if present, makes the checkbox checked by default.

Unchecked by default:
```html
<input type="checkbox" name="isReadyToEat" />
```

Checked by default:
```html
<input type="checkbox" name="isReadyToEat" checked />
```

To conditionally render the checked attribute based on the `fruit.isReadyToEat` value, we'll use EJS control flow logic in the checkbox input:

```html
<input type="checkbox" name="isReadyToEat" <% if (fruit.isReadyToEat) { %>checked<% } %> >
```

This setup ensures that the checkbox reflects the current state of the fruit. If `fruit.isReadyToEat` is true, the checked attribute is added, and the checkbox will be checked when the page loads. Otherwise, it remains unchecked.

Test your `edit` UI on fruits that are both "ready to eat" and "not ready to eat". Create some extra fruits if necessary so you have both kinds.

Testing the form submission will result in an error, `Cannot PUT /fruits/<fruitID>`. 

We'll fix that in the next section.
