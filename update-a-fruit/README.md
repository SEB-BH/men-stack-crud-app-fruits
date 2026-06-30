<h1>
  <span class="headline">Fruits</span>
  <span class="subhead">Update a Fruit</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a route that will receive the submission of the form in `/fruits/edit`. This route will update a fruit document in the `fruits` collection in their MongoDB Atlas database.

## The `update` route

In the next section, we will focus on building the `update` route. This route is the final component in completing the CRUD (Create, Read, **Update**, Delete) functionalities of our application. The `update` route is responsible for processing the data submitted from the `edit` form and applying those changes to the corresponding item in the database.

In keeping with RESTful routing conventions, the url for this route will be `/fruits/:fruitId`

## Create the route

To allow users to `update` a fruit's details, we need to set up an `update` route. This route will handle `PUT` requests sent from the edit form on the `Edit` page. The URL pattern for this route is `/fruits/:fruitId`, where `:fruitId` represents the unique identifier of the fruit being updated.

Like the `create` route, the `update` route requires special data handling for the `isReadyToEat` property. This property, being a checkbox in our form, sends different values based on whether it's checked or not, and we need to handle this in our route to ensure it aligns with our database schema.

The `update` route is unique as it specifically deals with `PUT` requests. Since it does not conflict with other routes, its placement within `server.js` can be flexible. However, it should be positioned after any middleware and before the `app.listen()` function.

In this route, we will employ the Mongoose `findByIdAndUpdate()` method. This method allows us to update a specific document in the MongoDB database based on its unique `ID`. Since this is an asynchronous operation, we'll utilize `async/await` to ensure the operation completes before proceeding.

Let's add the following in `server.js`:

```js
// server.js

app.put("/fruits/:fruitId", async (req, res) => {
  // Handle the 'isReadyToEat' checkbox data
  if (req.body.isReadyToEat === "on") {
    req.body.isReadyToEat = true;
  } else {
    req.body.isReadyToEat = false;
  }
  
  // Update the fruit in the database
  await Fruit.findByIdAndUpdate(req.params.fruitId, req.body);

  // Redirect to the fruit's show page to see the updates
  res.redirect(`/fruits/${req.params.fruitId}`);
});
```
After updating the fruit in the database, it's a good practice to redirect the user back to a relevant page. In this case, we redirect the user to the `show` page of the updated fruit. This provides immediate visual confirmation of the changes.

We can now test the route by submitting our form. Make sure you are redirected correctly and can see your changes.

### You did it!

If you can see your newly updated fruit, you have successfully performed your final CRUD operation, **Update**. Congrats!

With this final route we have full CRUD and all seven RESTful routes on the fruit model! Well done! 🎉