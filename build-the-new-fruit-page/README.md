<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Build the New Fruit Page</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to construct a `new` route in Express that renders a form for collecting user data.

## The NEW Route

Creating a new fruit in our application involves two distinct steps, each handled by separate routes. The first step is presenting the user with a form to enter fruit data. This is the responsibility of the "new" route, which we'll build in this section. Its sole purpose is to display a form for data entry.

Once the user fills out the form and submits it, the data is sent to another route, which we'll construct in the next section. This second route is dedicated to processing the submitted data and inserting it into the database.

## Define the route

In keeping with RESTful routing conventions, the url for this route will be: `/fruits/new`.

Let's start by defining the route and testing it. Add the following code to `server.js`, beneath the landing page route and above the `app.listen()` method (remember, all routes should be defined above `app.listen()`).

```javascript
// server.js

// GET /fruits/new
app.get("/fruits/new", (req, res) => {
  res.send("This route sends the user a form page!");
});
```

Test it by visiting `localhost:3000/fruits/new` in your browser.

## Create the new template

Let's now create the template for our "new" route. This template will provide a user-friendly interface for adding new fruits to our application. To keep our templates organized, especially as our application grows, we'll place this new template in a dedicated sub-folder within the `views` directory.

```bash
mkdir views/fruits
touch views/fruits/new.ejs
```

Organizing our templates into model-specific sub-folders is a good practice, especially for larger applications with multiple models. Each model can have its own `new.ejs` template, among others.

Now, let's add some basic content to our `new.ejs` template for fruits:

```html
<!-- views/fruits/new.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Create a Fruit</title>
  </head>
  <body>
    <h1>Create a New Fruit!</h1>
  </body>
</html>
```

To ensure best practice, we'll focus on testing each component as soon as it's developed. This approach not only helps in identifying and fixing issues early but also in understanding each part of the application thoroughly. So, before we dive into creating the user form, let's first update and test the route. 

## Updating the new route

Instead of `res.send`, let's render the `new.ejs` template we just created in the `views/fruit` directory:

```javascript
// server.js

// GET /fruits/new
app.get("/fruits/new", (req, res) => {
  res.render("fruits/new.ejs");
});
```

Visit `localhost:3000/fruits/new` in the browser. This time we should see the `<h1>` rendered to the page.

## Create the form

Finally, let's add a form to our `new.ejs` template. This form will allow users to input data for creating a new fruit. In our Fruit model, we have two properties: `name` and `isReadyToEat`. The `name` field should be a text input where users can type the name of the fruit. For the `isReadyToEat` property, which is a boolean, we'll use a checkbox input. This way, users can check the box if the fruit is ready to eat.

```html
<!-- views/fruits/new.ejs -->

<body>
  <h1>Create a New Fruit!</h1>
  <form action="/fruits" method="POST">
    <label for="name">Name:</label>
    <input type="text" name="name" id="name" />

    <label for="isReadyToEat">Ready to Eat?</label>
    <input type="checkbox" name="isReadyToEat" id="isReadyToEat" />

    <button type="submit">Add Fruit</button>
  </form>
</body>
```

In the form we've just created, there are two important attributes: `action` and `method`. The `action` attribute specifies the URL where the form data should be sent when the form is submitted. In our case, it's set to `/fruits`. The `method` attribute defines the HTTP `method` the browser should use to send the form data. We've set this to `POST`, which is appropriate for creating new data on the server.

Now that our form is set up, it provides the user interface needed to create a new fruit. However, it's not functional yet. When you try to submit the form, you'll probably see an error message like `Cannot POST /fruits`. This is because we haven't yet created the route to handle the form submission. That's our next step! We'll set up the corresponding server-side route to process and save the form data.