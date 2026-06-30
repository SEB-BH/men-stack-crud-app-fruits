<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Build and Run <code>server.js</code></span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a basic Express server and run it in their terminal.

## Install the Express Module

Before we begin building our server, let's use `npm` to install the Express module in this project:

```bash
npm i express
```

> 📚 Note that `i` is a shortcut for `install`.

Now that we've done this, the Express framework is available for us to use when building our application. Without this step, if we were to try to run our server code, we would receive an error since our app would be be trying to import a node module that we haven't installed yet.

Now that `express` is installed, let's write the basic code needed to create our server.

## Basic Structure of Express App

Here is a helpful outline of what a typical Express app does - let’s put this code right in our `server.js` file:

```js
// Here is where we import modules
// We begin by loading Express
const express = require('express');

const app = express();

app.listen(3000, () => {
  console.log('Listening on port 3000');
});
```

> 📚 The `server.js` file is typically the main entry point and configuration file for setting up an Express web server.

## Run the Server

Let's test our work by running our server. Run the following in your terminal:

```bash
nodemon
```

Our server should be up and running and you should see _`Listening on port 3000`_ logged to your terminal.
