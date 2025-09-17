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