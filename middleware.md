# Middleware

Middleware functions are functions that have access to the request object (req), the response object (res), and the next function in the application’s request-response cycle which is used to call the next function.
The next function is a function in the Express router which, when invoked, executes the middleware succeeding the current middleware.

The next() Function
If your middleware does not send a response (no res.send(), res.end(), etc.), you must call next() so that the next middleware or route handler can process the request.

If your middleware does send a response (e.g., res.json()), you shouldn’t call next() afterward, because the request lifecycle has already ended. Calling next() after sending a response can lead to errors like ERR_HTTP_HEADERS_SENT (headers already sent).

```js
app.use('/', (req, res, next)=>{
    console.log('m1');
//    next()
})

app.get('/', (req, res) => {
  res.send('<h1>Hello World!</h1>')
})
```

Since next() is commented out, the request will not proceed to the next handler (app.get('/')).

As a result, the client won't receive a response, and the request will hang indefinitely.

```js

// Example of a middleware getting the info of the date and method when a user is logged in.


app.use('/', (req, res, next)=>{
    fs.appendFileSync('log.txt', `The date of user loggin is ${new Date()} and the loggin method is ${req.method} \n`)
    req.something = 'I am a modified req which can be used anywhere'
    console.log('user logged in');
    next()
})

app.get('/', (req, res)=>{
    res.send('<h1>Hello World!</h1> ' + req.something)
})
```

Ordering is same as js:-

```js
app.use('/', (req,res,next)=>{
    req.some = "hello"
    next()
})
app.use('/', (req,res,next)=>{
    req.some = "no hello"
    next()
})

app.get('/', (req, res) => {
    res.send('<h1>Hello World!</h1> ' + req.some)
})
```

"no hello" will be printed as it is modified last, basic js syntax.


# Some types of middlewares

## Application-level middleware
Basic application level middleware

```js
const express = require('express')
const app = express()

app.use((req, res, next) => {
  console.log('Time:', Date.now())
  next()
})
```
## Router-level middleware
specific route middleware

```js
router.use((req, res, next) => {
  console.log('Time:', Date.now())
  next()
})
```

## Error-handling middleware
Used to handle errors

```js
app.use((err, req, res, next) => {
  console.error(err.stack) // prints the error details
  res.status(500).send('Something broke!')
})
```

## Built in middleware 
Provided by the express package, like:- 
express.static serves static assets such as HTML files, images, and so on.

```js
app.use(express.static('public'))
```

## Third-party middleware
Third-party middleware in Express.js refers to middleware functions that are not built into Express but instead come from external packages. These middleware modules are developed by the community and provide additional functionality that isn't natively available in Express.

The following example illustrates installing and loading the cookie-parsing middleware function cookie-parser.

```js
$ npm install cookie-parser


const express = require('express')
const app = express()
const cookieParser = require('cookie-parser')

// load the cookie-parsing middleware
app.use(cookieParser())
```