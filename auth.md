# app.use(express.json())
to get the data correctly
express.json() is built-in middleware (since Express 4.16). It parses incoming requests with Content-Type: application/json and populates req.body with the parsed JSON object.

```js
const express = require('express');
const app = express();

app.use(express.json()); // <-- here

// This means:
//Any incoming request with a JSON body will automatically be parsed.
//Instead of manually parsing req yourself, you can directly access req.body:


app.post('/data', (req, res) => {
  console.log(req.body); // already parsed JSON
  res.send('Received');
});
```
Without app.use(express.json()), req.body would be undefined for JSON payloads unless you use another body parser.

# validator package
to validate fields like email etc

```
npm install validator
```

```js
import validator from 'validator'

const newUser = {
    fullName: 'Marcus Aurelius',
    username: 'Marcus1',
    email: 'marcus@holy-roman-empire.org',
    password: 'Gladiators!'
}

console.log(validator.isEmail(newUser.email)) // true
```

# validation 

We will test if thee validation is good at the backend first not the frontend so that our app is more secure

some tasks of validation:
```js
/*
Challenge:
1. Validate the incoming user data.
  - Make sure all fields are present.
  - Remove any whitespace where appropriate.
  - Use regex /^[a-zA-Z0-9_-]{1,20}$/ to check the username contains only the allowed characters.
  - Use the Validator package to check the email format is valid.

If fields are not present, the username uses disallowed characters, or the email address is not of a valid format, end the response with a suitable code and send an error object with a suitable message. For example:
   { error: 'All fields are required.' }
  - Test with console.logs.
*/

import validator from 'validator'

export async function registerUser(req, res) {
  let { name, email, username, password } = req.body

  if ( !name || !email || !username || !password ) {
    return res.status(400).json({ error: 'All fields are required.' })
  }

  name = name.trim()
  email = email.trim()
  username = username.trim()

  if (!/^[a-zA-Z0-9_-]{1,20}$/.test(username)) {
    return res.status(400).json({ error: 'Username must be 1–20 characters, using letters, numbers, _ or -.' })
  }

  if (!validator.isEmail(email)) {
    return res.status(400).json({ error: 'Invalid email format' })
  }
}

// some frontend validation
<input 
    type="text" 
    id="signup-username" 
    name="username"
    pattern="^[a-zA-Z0-9_-]{1,20}$" 
    title="Username must be 1–20 characters and can only include letters, numbers, underscores (_), orhyphens (-)."
    required
/>
```


## database validation 

some tasks:-
```js
// db.js
import sqlite3 from 'sqlite3'
import { open } from 'sqlite'
import path from 'node:path'

export async function getDBConnection() {

const dbPath = path.join('database.db')
 return open({
   filename: dbPath,
   driver: sqlite3.Database
 }) 
} 


// auth.js


import { getDBConnection } from '../db/db.js'

  try {
    const db = await getDBConnection()
/*
Challenge:
1. Check if the username or email address has already been used.
  - If it has, end the response with a suitable status code and this object:
    { error: 'Email or username already in use.' }.

  - If the username and email address are unique in the database, add the user to the table and send thisJSON { message: 'User registered'}. Which status code should you use?
- You will be able to see the password in the db!
*/


    const db = await getDBConnection()
    const existing = await db.get('SELECT id FROM users WHERE email = ? OR username = ?', [email, username])

    if (existing) {
      return res.status(400).json({ error: 'Email or username already in use.' })
    }

    const result = await db.run('INSERT INTO users (name, email, username, password) VALUES (?, ?, ?, ?)', [name, email, username, password])
    res.status(201).json({ message: 'User registered'})

} catch (err) {

  console.error('Registration error:', err.message);
  res.status(500).json({ error: 'Registration failed. Please try again.' })
}

// j=here passwords are stored as plain text which is very wrong
```

# hashing
Hashing is the process of transforming data into a fixed-length, irreversible string of characters that uniquely represents the original input.
We hash a password into a fixed-length string which can not be decoded.
but it is not enough on it's own as even though it can't be reversed back into a string but hackers have something called a rainbow table.

## rainbow table
A precomputed table of hashes for many possible passwords.
Attackers use it to quickly reverse-engineer hashed passwords by looking up their hash instead of brute-forcing.

## Salting
to fix these we use something called salting(
Add a unique, random string (“salt”) to each password before hashing.
This ensures the same password hashed by two users produces different hashes.
It also makes precomputed rainbow tables useless because the attacker would have to build a table for every possible salt.
)

Hash + salt:-
Create a salt, a random string like a uuid(Universally Unique Identifier)
Combines salt and password
Runs the hash algorithm
Outcome: a super unique string!
ex:-
Password: OrangeJuice66
```
$2b$10$eImiTXuWVxfM37uY4JANjQyYFf3D9bXjAFnJPI7XyExNpPOD6UnmG
```
Algorithm: $2b
Cost factor: 10
Salt: eImiTXuWVxfM37uY4JANjQ
Stored hash: yYFf3D9bXjAFnJPI7XyExNpPOD6UnmG


this is not 100% secure but very difficult and incredibely expensive to breach,if we add in some rate limitng(Rate limiting is a security and performance technique used to control how many requests a user, IP address, or client can make to a server in a given period of time.) it can take upto years to get a password.
also can use two factor authentication for extra layer of security.

## Signup Flow
User provides password →

Bcrypt:
Generates random salt
Hashes password + salt →

Hashed and salted password stored in database

## login flow
User inputs password →

Original password hash retrieved from database →

Bcrypt:
Takes salt from original password, adds it to the new incoming password & hashes it.

Compares new hash to original hash in database. →

If they match, user is signed in. If not, sign in fails.

# Bcrypt

```js
const hashedPassword = await bcrypt.hash(password, costFactor);
```

The cost factor (also called work factor) in bcrypt controls how computationally expensive the hashing is.
Higher cost = slower hash generation, which makes brute-force attacks much harder but also uses more server CPU time.
A pretty standard for hash factor is 10

## Signup

```js
import express from 'express'
import bcrypt from 'bcryptjs'

const app = express()

const password = 'skywalker96'
const hashed = await bcrypt.hash(password, 10)

console.log(hashed)

app.listen(8000, () => console.log('listening 8000'))
```

## Login

```js
const userInDb = {
  name: 'Luke Skywalker',
  password: '$2b$10$pyzxk19lHZbx/aGmjbVGeOswnmX7lwxnDg2RQUhnOSMhFeRYaXtSq'
} 

const loginAttempt = {
  name: 'Luke Skywalker',
  password: 'skywalker96'
}

const userIsValid = await bcrypt.compare(loginAttempt.password, userInDb.password)
console.log(userIsValid) // true 
```

# Session
HTTP is stateless, meaning each request from a client to a server is independent and contains no knowledge of previous interactions.

The express-session package provides a way to store user-specific data, such as login state, between HTTP requests.

## Login flow
User submits credentials: The user provides their username and password.

Server authenticates user: The server verifies the credentials against its records.

Session is created or updated: If the credentials are valid, a new session is created or an existing one is updated for the user.

Session ID is sent to client: The server sends a unique session ID back to the client, usually as a cookie.

Client stores session ID and sends with all requests: The client's browser stores the session ID and automatically includes it in the headers of all subsequent requests to the server.

Server restores session on each request: The server uses the received session ID to look up and restore the user's session data.

User is now logged in: The user is now authenticated and can access protected resources.

## Development and Production
In development, you typically use express-session with the default MemoryStore. This is sufficient for local testing as the session data is stored in the server's process memory. However, this is not suitable for a production environment.

In a production environment, you need a persistent and scalable session store. The default MemoryStore has several critical limitations:

No persistence: If your server restarts, all session data is lost, forcing users to log in again.
No horizontal scaling: Session data is tied to a single server instance. If you use multiple servers (e.g., in a load-balanced setup), a user's subsequent request might hit a different server that has no knowledge of their session, breaking the login flow.

To address these issues, use an external, production-ready session store that can be accessed by all your application servers. The most common choices are:

Redis: An in-memory data store known for its high performance and low latency. It is a popular choice for session management due to its speed.

PostgreSQL/MySQL: Relational databases can also be used, but they are generally slower for high-frequency key-value lookups compared to Redis. They can be a good option if you already have a relational database in your stack and want to avoid adding another technology.

MongoDB: A document database that works well for storing session data, especially if you're already using it for your application's data.

# Express-session
by default, express-session uses an in-memory session store called MemoryStore. This means that all session data is stored in the RAM of the server process.


```
npm install express-session
```

```js
import session from 'express-session'

const secret = process.env.SPIRAL_SESSION_SECRET // acts as a signature to look for modified id and rejects it


app.use(session({
  secret: secret,
  resave: false, // don't save the data again and again on every req
  saveUninitialized: false, // prevents empty session to be stored, every new user will get a session even if they don't login or interact.
  cookie: {
    httpOnly: true, // makes the cookie inaccessible to the javascript
    secure:false, // if it was true, will tell the browser to only send the cookie over https not only http(in local host http is used not https as in development we are setting it to false).
    sameSite: 'lax' //security attribute for web cookies that restricts when a browser sends cookies with cross-site requests.
  }
}))
```

we can use the last id attribute that is given to us by sqlite 3 to bind the loggedin user to the session.

```js

  const result = await db.run('INSERT INTO users (name, email, username, password) VALUES (?, ?, ?, ?)', [name, email, username, hashed])
  console.log(result)

  req.session.userId = result.lastID
```

# logout

```js
export async function logoutUser(req, res) {

  req.session.destroy( () => {

    res.json({ message: 'Logged out' })

  })

}
```

# Protect Routes

```js

// requireAuth.js
export function requireAuth(req, res, next) {

  if (!req.session.userId) {

    console.log('Access to protected route blocked')
    return res.status(401).json({ error: 'Unauthorized' })

  }
  next()
}


// import and use it as midleware on any route that you may want to protect

// cart.js
cartRouter.post('/add', requireAuth, addToCart) 
cartRouter.get('/cart-count', requireAuth, getCartCount)
cartRouter.get('/', requireAuth, getAll) 
cartRouter.delete('/all', requireAuth, deleteAll) 
cartRouter.delete('/:itemId', requireAuthdeleteItem) 



// additional logic that checks and redirects the user if not logged in

  if (!res.ok) {
    window.location.href = '/'
    checkoutBtn.disabled = true
    checkoutBtn.classList.add('disabled')
    userMessage.innerHTML = 'Please <a href="login.html">log in</a>.'
    return []
  }

// if (!res.ok): This is the main condition. The res object likely represents a response from an API call (e.g., using fetch). The ok property is a boolean that is true if the HTTP response status code is in the range 200-299, and false otherwise

// window.location.href = '/': This line redirects the user to the root URL of the website.

//checkoutBtn.disabled = true: This line disables a button element with the ID checkoutBtn, preventing the user from clicking it again.


```