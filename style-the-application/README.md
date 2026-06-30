<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Style the Application</span>
</h1>

**Learning objective** : By the end of this lesson, students will be able to enhance the user experience of their full-stack applications by effectively integrating CSS styling through the use of static files.

Now that we've got a fully functional app, it's time to add some styling! These steps are divided into labelled sections, so feel free to follow along for as much or as little as you'd like. If you do choose to complete all the sections, you'll have a fully styled app by the end of this level up.

## Setting up the styleshets

1. Create the following folders in your project directory: `public` and `public/stylesheets`:

    ```bash
    mkdir public public/stylesheets
    ```

2. In your `server.js` file, add the following middleware to serve static files from the directory:

    ```javascript
    app.use(express.urlencoded({ extended: false }));
    app.use(methodOverride("_method"));
    // app.use(morgan('dev'));

    // new code below this line
    app.use(express.static(path.join(__dirname, "public")));

    // new code above this line
    app.get("/", async (req, res) => {
      res.render("index.ejs");
    });
    ```

3. Require the `path` module at the top of your `server.js` file:

    ```js
    const methodOverride = require("method-override");
    const morgan = require("morgan");
    // new code below this line
    const path = require("path");
    ```

4. Create a CSS a `style.css` file within the `stylesheets` folder:

    ```bash
    touch public/stylesheets/style.css
    ```

## Basic Styling

1. Link the `style.css` file to all of your ejs pages by adding the following code within the `<head>` section of your pages:

    ```html
    <!-- all template pages -->

    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />

      // new code below this line
      <link rel="stylesheet" href="/stylesheets/style.css" />
    </head>
    ```

2. Import our font from Google Fonts into `style.css` at the very top of the stylesheet:

    ```css
    @import url("https://fonts.googleapis.com/css2?family=DM+Sans:opsz@9..40&display=swap");
    ```

3. Add an image to the `index.ejs` page:

    ```html
    <!-- index.ejs homepage -->

    <body>
      <h1>Welcome to the Fruits app!</h1>

      // new code below this line
      <img
        class="fruit-pic1"
        src="https://cdn-icons-png.flaticon.com/128/5312/5312448.png"
        alt="happy fruit salad bowl"
      />
      //new code above this line

      <p>An app for collecting your favorite fruits.</p>
    </body>
    ```

4. Add a footer to the bottom of `index.ejs`, under the closing `</body>` tag (we'll use this to attribute the icon we're using for this app):

    ```html
    <!-- index.ejs homepage -->

    </body>
    // new code below this line
    <footer>Icons by <a href="https://www.flaticon.com/authors/freepik">Freekpik</a></footer>
    ```

5. Add the following basic styles to style.css:

    ```css
    body {
      background: #fff8ed;
      font-family: "DM Sans", sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      max-width: 100vw;
    }

    h1 {
      color: #769642;
      font-size: 3.6em;
    }

    footer {
      font-size: 0.7em;
      position: fixed;
      left: 0;
      bottom: 0;
      width: 100%;
      text-align: center;
      padding: 10px;
    }

    .fruit-pic1 {
      height: 30%;
    }
    ```

## Styling links

1. Modify the `<a>` tag on `index.ejs` to include a class called `fruits-index-link`:

    ```html
    <!-- index.ejs fruits -->

    <a class="fruits-index-link" href="/fruits">Browse Fruits</a>
    ```

2. Wrap the text of that same `<a>` tag in a `<div>` with the class name of `index-link-text`. Do this for all links in the app that say "Back to Fruits" (on the `fruits/edit.ejs` and `fruits/show.ejs` pages):

    ```html
    <!-- edit.ejs / show.ejs -->

    <a class="fruits-index-link" href="/fruits">
      <div class="index-link-text">Back to Fruits</div>
    </a>
    ```

3. Add the following style rules to `style.css`:

    ```css
    .fruits-index-link {
      text-decoration: none;
    }

    .index-link-text {
      background-color: #f47b5b;
      color: white;
      font-size: 1.4em;
      width: 10em;
      padding: 5px 15px;
      margin: 15px;
      border-radius: 25px;
      text-align: center;
      box-shadow: 0 5px #ffbd59;
    }

    .index-link-text:hover {
      background-color: #ffbd59;
      box-shadow: 0 5px #f47b5b;
    }

    .index-link-text:active {
      background-color: #f47b5b;
      box-shadow: 0 2.5px #ffbd59;
    }
    ```

## Styling forms

1. On `new.ejs` and `edit.ejs`, add a `<div>` around the `<label>` and `<input>` for `ready to eat`, and apply the class `checkbox-div`:

    ```html
    <!-- new.ejs / edit.ejs -->

    <div class="checkbox-div">
      <label for="ready-to-eat">Ready to Eat?</label>
      <input type="checkbox" name="isReadyToEat" id="ready-to-eat" />
    </div>
    ```

2. Add the following CSS to `style.css`:

    ```css
    form {
      font-family: "DM Sans", sans-serif;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: flex-start;
      background-color: #ffbd59;
      padding: 20px 30px;
      border-radius: 20px;
      box-shadow: 5px 6px #f47b5b;
      margin: 20px 10px;
    }

    .checkbox-div {
      flex-direction: row;
    }

    label,
    input {
      margin: 10px 2px 0 2px;
    }

    button {
      align-self: center;
      margin: 10px 4px 4px 4px;
      background-color: #3f6a51;
      color: white;
      padding: 5px 10px;
      border-radius: 15px;
      border: none;
      width: 15em;
    }

    button:hover {
      background-color: #769642;
    }

    button:active {
      background-color: #2d4d3a;
    }
    ```

## Styling `show.ejs`

1. On `show.ejs` add a `<div>` around the content, buttons, and links, and apply the class `fruit-details`:

    ```html
    <!-- show.ejs -->

    <body>
      <div class="fruit-details">
        <!-- show page content -->
      </div>
      <a class="fruits-index-link" href="/fruits">
        <div class="index-link-text">Back to Fruits</div>
      </a>
    </body>
    ```

2. Add a second `<div>` around the `delete` form and `edit` link, and give it a class of `fruit-actions`:

    ```html
    <div class="fruit-actions">
      <form
        class="delete-form"
        action="/fruits/<%=fruit._id%>?_method=DELETE"
        method="POST"
      >
        <button class="delete-button" type="submit">
          Delete <%= fruit.name %>
        </button>
      </form>
      <a class="edit-link" href="/fruits/<%= fruit._id %>/edit">
        <div class="edit-text">Edit <%= fruit.name %></div>
      </a>
    </div>
    ```

3. Add the following CSS to `style.css`:

    ```css
    .fruit-actions {
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      width: 100%;
      height: 130px;
      padding: 20px 0;
    }

    .delete-form {
      padding: 0;
      box-shadow: 0 0;
      background-color: transparent;
      height: 50px;
      margin: 0 20px 0 0;
      min-height: 100%;
    }
    .delete-button {
      background-color: #f47b5b;
      text-transform: uppercase;
      padding: 40px 15px;
      width: 70%;
      /* height: 100%; */
      font-size: 1.3em;
      margin: 0;
      min-height: 100%;
    }

    .edit-link {
      text-decoration: none;
    }

    .edit-text {
      background-color: #3f6a51;
      text-transform: uppercase;
      padding: 40px 15px;
      font-size: 1.3em;
      border-radius: 15px;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      font-weight: bold;
      text-align: center;
      width: 70%;
      height: 50px;
    }

    .delete-button:hover {
      background-color: #ffbd59;
    }

    .delete-button:active {
      background-color: rgb(217, 108, 78);
    }

    .edit-text:hover {
      background-color: #769642;
    }

    .edit-text:active {
      background-color: #2d4d3a;
      background-color: rgb(217, 108, 78);
    }
    ```

## Styling `fruits/index.ejs`

1. Modify the HTML structure on `fruits/index.ejs` to create a header `<div>` and wrap the fruit names in cards:

    ```html
    <!-- index.ejs -->

    <body>
      <div class="index-header-div">
        <h1>All Fruits</h1>
        <a class="add-fruit" href="/fruits/new">Add fruit</a>
      </div>
      <ul>
        <% fruits.forEach((fruit)=> { %>
        <li>
          <a class="fruit-cards" href="/fruits/<%=fruit._id%>">
            <%= fruit.name %>
          </a>
        </li>
        <% }) %>
      </ul>
    </body>
    ```

2. Add class name to the `Add fruit` link called `add-fruit`:

    ```html
    <a class="add-fruit" href="/fruits/new">Add fruit</a>
    ```

3. Add the following styles to `style.css`:

    ```css
    /* index styling */

    ul {
      display: flex;
      flex-wrap: wrap;
    }

    li {
      list-style: none;
    }

    .fruit-cards {
      display: flex;
      flex-direction: column;
      text-decoration: none;
      color: black;
      width: 40px;
      height: 70px;
      background-color: white;
      margin: 10px 15px;
      padding: 30px 50px;
      border-radius: 15px;
      align-items: center;
      justify-content: center;
      border: 2.5px solid #ffbd59;
    }

    .fruit-cards:hover {
      border: 4px solid #f47b5b;
    }

    .fruit-cards:active {
      background-color: #a4cf5f67;
      border: 2.5px solid #ffbd59;
      color: gray;
    }

    .index-header-div {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      width: 98%;
    }

    .add-fruit {
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0 20px;
      text-decoration: none;
      width: 6em;
      text-transform: uppercase;
      background-color: #769642;
      border-radius: 30px;
      color: white;
      padding: 10px;
      font-weight: bold;
      font-size: 1.2em;
    }

    img {
      height: 100%;
      margin: 20px 0 0 0;
    }
    ```

Congrats! You have a fully styled app!
