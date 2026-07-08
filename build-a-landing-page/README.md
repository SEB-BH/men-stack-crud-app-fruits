<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Build a Landing Page</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to implement a landing page for a web application using Express and EJS.

## The Landing Page

Every web application typically features a landing page, serving as the initial point of entry for users. When someone navigates to *"mycoolwebapp.com"*, this page is the first they encounter, offering links or navigation to explore other sections of the application. In this project, we'll be using EJS (Embedded JavaScript) templates to create the HTML views for this landing page and other parts of the web application.

## Install `ejs` from NPM

We'll need the `ejs` package from NPM so that we're able to deliver views, let's add it to our project. Stop your server and install the following package:

```bash
npm i ejs
```

We don't need to require it in our `server.js` file because express knows how to find this package out of the box.

## Create a `views` directory

We're going to need a `views` directory to hold all of our templates. Let's create it now:

```bash
mkdir views
```

Our landing page will be an `home.ejs` file that is inside of this directory. Let's create that too:

```bash
touch views/home.ejs
```

Open `home.ejs` and add the following HTML boilerplate and content inside of the `<body>` tag. Don't forget to also change the `<title>`!

```html
<!-- views/home.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Fruits App</title>
  </head>
  <body>
    <h1>Welcome to the Fruits app!</h1>
    <p>An app for collecting your favorite fruits.</p>
  </body>
</html>
```

To show our homepage when a user visits the root `/` of our application, we need to modify the response in the server's route handler. Instead of sending a simple message with `res.send()`, we'll use the `res.render()` method. This method allows us to render our EJS template as HTML.

```javascript
// server.js

// GET /
app.get("/", async (req, res) => {
  res.render("home.ejs");
});
```

Browse to `localhost:3000` and see the result!