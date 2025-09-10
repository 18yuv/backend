# Express js
documentation for [Express js](https://expressjs.com/)

Express.js is a fast, minimal, and flexible web framework for Node.js that simplifies server-side development. It provides a set of powerful features for building web applications and APIs.

In Node.js, when you create a module, the code inside that module is automatically wrapped in a function by Node.js itself. This function provides some useful local variables to every module, such as:

```js
(function(exports, require, module, __filename, __dirname) {
    // Your module code is inside this function
})();
```


initialise as a npm project 

```
npm init -y
```

install express
```
npm i express
```

documentation for express js 
https://expressjs.com/en/starter/hello-world.html 


# nodmeon
to automatically start the server and avoid the inconvinince use 

```
npm i --global nodemon
```

now just type 

```
nodemon /fileName
```

or you can use node also:-

```
node filename 
```
You have to restart the server again and again if you are using this.

# hello world
require() is a built-in function used to import modules. It allows you to bring in external libraries.
Loads a module and makes its functionality available in your code.

```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

or change the type to be module and proceed :-

```js
import express from "express"

const PORT = 8000

const app = express()

app.listen(PORT, ()=>{
    console.log(`server connected on port ${PORT}`);
})
```

run by typing npm start

## for not hard coding a path 

use : for not hard coding a path param :-
```js
/api/contry-name/:currency

// this will let us access the currency that the user puts ex: btc, eth 
```

# express.static

To access public files, create a folder (public named folder is the standard) and list the files there then:-

```js
app.use(express.static('public'))
```

# Post req

we can use the traditional way of testing our post reqs for testing api's by making an html page or we can use postman.

```js
app.post('/', (req,res)=>{
    res.send('<h1>Its a post req</h1>')
})

<script>
        (async function testPost() {
            let a = await fetch("/", {method : "POST"});
            let b = await a.text();
            console.log(b);
        })();
</script>
```

# express.Router

Express.Router() is a mini Express application that allows you to create modular, mountable route handlers. It helps organize routes into separate files and makes large applications more manageable.
different pages ke liye diffrent files mein code karo, makes code more manageble and readable.

```js
// inside routes/userRoutes folder

const express = require('express');
const router = express.Router();

// Define routes within the router
router.get('/', (req, res) => {
    res.send('User List');
});

router.get('/:id', (req, res) => {
    res.send(`User ID: ${req.params.id}`);
});

module.exports = router;



// main.js
const express = require('express');
const app = express();
const userRoutes = require('./routes/userRoutes'); // Import user routes

app.use('/users', userRoutes); // users wale endpoint ko userRoutes folder handle karega
```