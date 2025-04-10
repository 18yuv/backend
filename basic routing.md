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

```
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

## POST

In Express.js, a POST request is used when a client sends data to the server, usually to create or update resources.

How It Works:
A client sends a POST request with data (like form submissions or JSON payloads).

The server processes the request and responds accordingly (e.g., storing the data in a database).

```
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

```
app.put('/user', (req, res) => {
  res.send('Got a PUT request at /user')
})
```


## DELETE

A DELETE request in Express.js is used to remove a resource from the server. It tells the server to delete a specific piece of data, such as a user, post, or database entry.

How It Works:
The client sends a DELETE request to a specific route.

The server identifies the resource to be deleted.

The server removes the resource and responds with confirmation.

```
app.delete('/user', (req, res) => {
  res.send('Got a DELETE request at /user')
}) 
```

## Method Chaining
You can chain these together if you want to:-

```
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