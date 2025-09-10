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

# express.Router

Express.Router() is a mini Express application that allows you to create modular, mountable route handlers. It helps organize routes into separate files and makes large applications more manageable.
different pages ke liye diffrent files mein code karo, makes code more manageble and readable.

```js
// userRoutes.js
import express from 'express';
const router = express.Router();

// Define routes specific to this router
router.get('/', (req, res) => {
    res.send('User home page');
});

router.get('/profile', (req, res) => {
    res.send('User profile page');
});

export default router; // export the router


// app.js
import express from 'express';
const app = express();
const port = 3000;
import router from './userRoutes.js';

// Use the router with a base path
app.use('/users', router);

// Other routes or middleware for the main app
app.get('/', (req, res) => {
    res.send('Main application home page');
});

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

# CORS

to make your api open to public you need to allow cross origin resource sharing, which is pretty easy to deal with in express:
```
nnpm install cors
```

```js
import cors from 'cors'
app.use(cors()) // apply at the top to allow everyone to access to all the routes 
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

# SQLite
SQLite is a relational database management system (RDBMS) that is different from other databases like MySQL or PostgreSQL because it's "serverless." Instead of a separate process running a server that your application connects to, the SQLite database is a C-language library that is embedded directly into your application. 

SQLite3 - The database driver
Opens a connection to the database file.
Executes SQL queries.
Handles reading and writing results.

SQLite - a wrapper
Provides async/await support for cleaner code.

```js
// create table .js
import sqlite3 from 'sqlite3'
import { open } from 'sqlite'
import path from 'node:path'

async function createTable() {

  const db = await open({
    filename: path.join('database.db'),
    driver: sqlite3.Database
  })

  await db.exec(`
  CREATE TABLE IF NOT EXISTS abductions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    location TEXT NOT NULL,
    details TEXT NOT NULL
  )
  `)

  await db.close()
  console.log('Table abductions created')
}

createTable()


// logtable.js
import sqlite3 from 'sqlite3'
import { open } from 'sqlite'
import path from 'node:path'


export async function viewAllProducts() {
  const db = await open({
    filename: path.join('database.db'), 
    driver: sqlite3.Database
  });

  try {
    const abductions = await db.all('SELECT * FROM abductions')
    console.table(abductions) // consoles a table 
  } catch (err) {
    console.error('Error fetching products:', err.message)
  } finally {
    await db.close()
  }
}

viewAllProducts()
```

some methods:

db.exec : multiple statement at once and schema setup anf no return data for us
```js
await db.exec('
    CREATE TABLE products (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        type TEXT NOT NULL,
        size TEXT NOT NULL,
    );
``` 

db.run :
to run single statement , used for inserting, updating, deleting data, no return for us
```js
db.run(
    'INSERT INTO users (name, email) VALUES (?, ?)',
    [name, email]
);
```

db.get: used when you expect one row back (or only the first row), lookup one row by its id 
```js
db.get('SELECT * FROM users WHERE id = ?', [id]) // returns the row to us
```

db.all: you want all matching rows form a table in the form of an array, use case like all out of stocks products, this also returns rows for use (in an array)
```js
db.all(
    'SELECT * FROM products WHERE status = ?',
    ['out_of_stock']
)
```

## insertion in sqlite

```js
db.run(
    'INSERT INTO users (name, email) VALUES (?, ?)',
    [name, email]
);
```

this line will run for each object when used in a loop for data in the database which would be slow, but we can do all of this in a single transaction by using exec:
```js
await db.exec('BEGIN TRANSACTION')

    for (const {location, details} of abductionsData) {
      await db.run(
        `INSERT INTO abductions (location, details)
        VALUES (?, ?)`,
        [location, details]
      )
    }
    
    await db.exec('COMMIT') // will end the transaction
```


to handel error in the insertion process:
```js
await db.exec('ROLLBACK')

// this ensures nothing is inserted if an error occurs, so basically everything is inserted or nothing is inserted.


// remeber to close the database connection in the finally block
```