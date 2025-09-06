# app.METHOD(PATH, HANDLER)

Where:

app is an instance of express.
METHOD is an HTTP request method, in lowercase.
PATH is a path on the server.
HANDLER is the function executed when the route is matched.

## GET

In Express.js, a GET request is used to retrieve data from a server. It's one of the most common HTTP request methods.

How It Works:
A client (like a browser or Postman) sends a GET request to a specific route (URL).

The server processes the request and sends back a response.

```js
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

## POST

In Express.js, a POST request is used when a client sends data to the server, usually to create or update resources.

How It Works:
A client sends a POST request with data (like form submissions or JSON payloads).

The server processes the request and responds accordingly (e.g., storing the data in a database).

```js
app.post('/', (req, res) => {
  res.send('Got a POST request')
})
```

## PUT 

A PUT request in Express.js (and in general web development) is used to update an existing resource on the server.

How It Works:
A client sends a PUT request with the updated data.

The server finds the existing resource and replaces it with the new data.

The response usually confirms that the update was successful.

```js
app.put('/user', (req, res) => {
  res.send('Got a PUT request at /user')
})
```

## PATCH
app.patch() is a method used to define a route handler for HTTP PATCH requests. PATCH requests are typically used to update partial data of a resource rather than replacing the entire resource, which is what a PUT request does.

```js
app.patch('/user/:id', (req, res) => {
    const userId = req.params.id;
    const updatedData = req.body;

    // Here, you would update the user data in your database
    res.send(`User ${userId} updated with ${JSON.stringify(updatedData)}`);
});
```

## Difference bwtween POST, PUT, PATCH

POST: Used to create a new resource. When you send a POST request to an endpoint, you're asking the server to generate and store new data. Example: Creating a new user.

PUT: Used to completely replace an existing resource. If you send a PUT request, the server will overwrite the entire resource with the new data you provide. Example: Updating a user’s profile where you must send all attributes, even unchanged ones.

PATCH: Used to partially update an existing resource. Unlike PUT, PATCH allows modifying only the fields that need to be changed without affecting others. Example: Updating just the user's email address while leaving the rest of the profile untouched.

POST creates a brand-new document.

PUT rewrites the entire document.

PATCH edits just specific sections without changing the rest.


## DELETE

A DELETE request in Express.js is used to remove a resource from the server. It tells the server to delete a specific piece of data, such as a user, post, or database entry.

How It Works:
The client sends a DELETE request to a specific route.

The server identifies the resource to be deleted.

The server removes the resource and responds with confirmation.

```js
app.delete('/user', (req, res) => {
  res.send('Got a DELETE request at /user')
}) 
```

## USE

app.use() in Express is a way to tell the server to "use" middleware functions. Middleware functions run before final route handlers and help process requests.
Route handlers in Express are functions that define what happens when a request reaches a specific route. They're part of routing, which controls how your server responds to different URLs and HTTP methods.
```js
const express = require('express');
const app = express();

// Middleware function
app.use('/middleware', (req, res, next) => {
    console.log('Middleware executed');
    next(); // Moves to the next function
});
```

## Method Chaining
You can chain these together if you want to:-

```js
app.route("/user")
   .get((req, res) => {
       res.send("Fetching user data...");
   })
   .post((req, res) => {
       res.send("Creating a new user...");
   })
   .put((req, res) => {
       res.send("Updating user information...");
   })
   .delete((req, res) => {
       res.send("Deleting user...");
   });
```


## codes :-

200 - ok
400 - bad request
404 - not found
500 - server error
201 - resource accepted to the server 

## content type :-
any data that is being passed or recieved needs to be parsed, specifying the content type helps the client and the server to do that, faliure causes bugs.

application/json
text/html
text/css
application/javascript
application/xml