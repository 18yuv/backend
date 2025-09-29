# SQLite
SQLite is a relational database management system (RDBMS) that is different from other databases like MySQL or PostgreSQL because it's "serverless." Instead of a separate process running a server that your application connects to, the SQLite database is a C-language library that is embedded directly into your application. 

SQLite3 - The database driver
Opens a connection to the database file.
Executes SQL queries.
Handles reading and writing results.

SQLite - a wrapper
Provides async/await support for cleaner code.

## Create table

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
```

## Log table

```js
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

db.exec : multiple statement at once and schema setup and no return data for us
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

all together:-
```js
try {

    await db.exec('BEGIN TRANSACTION')

    for (const {location, details} of abductionsData) {
      await db.run(
        `INSERT INTO abductions (location, details)
        VALUES (?, ?)`,
        [location, details]
      )
    }
    
    await db.exec('COMMIT')
    console.log('All records inserted')

  } catch (err) {

    await db.exec('ROLLBACK')
    console.log('Error inserting data', err.message)

  } finally {

    await db.close()
    console.log('connection closed')

  }
```

Efficient way to insert everything
```js
    const query = 'SELECT * FROM abductions WHERE location = ?'
    const params = ['Roswell']

    const abductions = await db.all(query, params)
```

## Search by params:-
```js
    const db = await getDBConnection()

    let query = 'SELECT * FROM products'
    let params = []

    const { genre } = req.query

    if (genre) {
      query += ' WHERE genre = ?'
      params.push(genre)
    }
 
    const products = await db.all(query, params)
```

## search functionality:-
```js

    const { search } = req.query

    if (search) {
      query += ' WHERE title LIKE ? OR artist LIKE ? OR genre LIKE ?'
      const searchPattern = `%${search}%` // partial search (lo would match block, slow, etc)
      params.push(searchPattern, searchPattern, searchPattern)
    }
```