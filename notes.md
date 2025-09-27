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


# CORS

To make your api open to public you need to allow cross origin resource sharing, which is pretty easy to deal with in express:
```
nnpm install cors
```

```js
import cors from 'cors'
app.use(cors()) // apply at the top to allow everyone to access to all the routes, just bellow where app is initialised.
```

to let people access only for a trusted domain :
```js
const corsOptions = {
  origin: 'https://www.my-frontend-app.com' // ⬅️ Only this origin is allowed
};

app.use(cors(corsOptions)); // ⬅️ Pass the configuration to the middleware
```

do i need to use cors at the very top?
Generally, yes, you should place the CORS middleware at the very top of your Express.js application, right after your initial imports and app instantiation. This ensures that the CORS headers are set and checked for all incoming requests, regardless of the route they are trying to access.