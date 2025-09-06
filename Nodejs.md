# package.json
contains all the meta data like name, version, author etc.
and also contians dependencies that can be further installed easily.

# initialise

```js
npm init
```

change type in js to "type": "module"
to run commands like import instead of reqiure


# http 

```js
import http from 'node:http'
```
use node: to specify that the module we are looking for is the core node madule rather than a javascript module made by ourselves.

Allows data to be transfered over the http module
create servers
handle request from clients
provide responses to those requests

A Basic node js Server:-

```jsx
import http from 'http';

const PORT = 8000; // port


// test this on postman
const server = http.createServer((req, res)=>{
    res.end("hi i am the body") // gets a response and then ends it 

    // res.write(chunk) → writes data to the response body. there is no res.send in node js
    // res.end ends the cycle so if you want to set headers or status code, do it above the res.end
})

server.listen(PORT, ()=>{
    console.log(`server is running on port ${PORT}`);
})
```

http is a text based protocol, all data transferred between the server and the client must be in string format.
use json.stringify(dataToConvert)
JSON.stringify(data, null, 2); // to prettify it

# req object

gives us access to the incoming request:-
thr url client used, use req.url
the headers
any data sent
the method(get, post, delete), req.method

# res object

tools like res.setHeader to define the content type
```js
res.setHeader('Content-Type', 'application/json');
```

tool like status code to set the code (ex 404)
```js
res.statusCode = 200;
```

to write some code in one line like status code and header  
```js
res.writeHead(200, {'Content-Type': 'text/html'})
```
this is not used much because no modification can be done as it sends the headers immediately.

```js
// an example sending response function
export function sendResponse(res, statsuCode, conentType, payload) {

  res.statusCode = statsuCode
  res.setHeader('Content-Type', conentType)
  res.end(payload)
  }
```

# path parameter

Path parameters are variables that are part of the URL path itself.
They let you capture values from the URL and use them inside your server logic.

They’re different from query parameters (?key=value) because they’re part of the path structure instead of tacked on at the end.

```
/api/country/france
/api/country/japan
```

```js
// get the endpoint
const country = req.url.split('/').pop()
const filteredData = destinations.filter((destination) => {
    return destination.country.toLowerCase() === country.toLowerCase()
})
```

# query parameter

Query parameters are values that you pass to a URL after a ?.
They are structured as key=value pairs, and multiple ones are separated by &.

```
/api/country?name=france&lang=fr
```


to use the key value pairs of the query we will first convert them into objects
```js
const urlObj = new URL(req.url, `http://${req.headers.host}`) 

// req.url stores the path and query string not the full url, to use new URL we need full path so we combine
// req.headers.host gives us the base url


const queryObj = Object.fromEntries(urlObj.searchParams) // this will convert the key value pairs into an object
urlObj.pathname // to checks
// JSON.parse() is a built-in JavaScript function that converts a JSON string into a JavaScript object (or array).
```

# Cross Origin Resourse Sharing (CORS)

CORS (Cross-Origin Resource Sharing) is a security feature built into web browsers.

It controls whether a web page running at one origin (domain + protocol + port) can make requests to a server at a different origin.

Same-origin request:
http://example.com → http://example.com/api/data ✅ allowed

Cross-origin request:
http://myapp.com → http://api.example.com/data 🚫 blocked by default


also doesn't works on different ports,
http://localhost:3000 -> http://localhost:3000
allowed

http://localhost:3000 -> http://localhost:8000
blocked

in, our sending response function we need to add:-
```js
res.setHeader('Access-Control-Allow-Origin', '*') // allow access from any origin 
res.setHeader('Access-Control-Allow-Methods', 'GET') // if we need only get method (May Vary)
```


# import.meta
used to get the files metadata about the module, but only can be used in modular js, ex type = module

# to get the current directery name 
process.cwd()

# paths

absolute path :- An absolute path is the complete path to a file or directory in a file system, starting from the root directory (the top level of the system).
ex-/home/user/Documents/report.txt

relative path :- A relative path is a way to specify the location of a file or folder in relation to your current working directory (the folder you’re currently "in").
ex-
../Pictures/photo.jpg

## path module
```js
import path from 'node:path'

// path.join joins path into a safe string
const abPath = path.join(import.meta.dirname, 'public', 'index.html') // this is an absolute path from root directory.
const relPath = path.join('public', 'index.html') // realtive path
```

# FS module

use to:-
readfiles, createfiles(writeFile), updatefiles, deletefiles, renameFiles


## Read file

```js
import fs from "node:fs"


const content = fs.readFileSync(pathToResource, 'utf8') // avoid serving your files through this unless you want your code to be synchronus.


// avoid using this method too as it can make the code more complex and could end in a callback hell
fs.readFile(pathToResource, 'utf8', (err, content) => {
if (err) {
    console.log(err)
    return
}
res.statusCode = 200
res.setHeader('Content-Type', 'text/html')
res.end(content)
})
```

A better way to use fs module would be using fs promises (also supports async await)
```js
import fs from 'node:fs/promises'

const content = await fs.readFile(pathToResource, 'utf8') // using encoding like utf8 is just optional but can interfere in serving images etc so not reccomended, but use setheader to interprete the serve files properly, ex(text/html)
```

## write file

Use the writeFile method with the following:
      - the relative path to the file 
      - The data (what should we do to this data first?)
      - The encoding 'utf8'

# global variable
```js
// import http from 'node:http' (module not commonJS)
const http = require('http')

console.log(__dirname)
console.log(__filename)
```

serve multiple files :-
```js
  const publicDir = path.join(__dirname, 'public')
  const pathToResource = path.join(
    publicDir, 
    req.url === '/' ? 'index.html' : req.url)


    const ext = path.extname(pathToResource) // to get the extentions like css because of the headers it only reads text/html not all types of extentions.



    export function getContentType(ext) {
   
   const types = {
     ".js": "text/javascript",
     ".css": "text/css",
     ".json": "application/json",
     ".png": "image/png",
     ".jpg": "image/jpeg",
     ".jpeg": "image/jpeg",
     ".gif": "image/gif",
     ".svg": "image/svg+xml"
   }

  return types[ext.toLowerCase()] || "text/html"



  // this way all the content types would be served
  const contentType = getContentType(ext)
  res.setHeader('Content-Type', contentType)
}
```

# chunk
When a server sends a response over HTTP, there are two main ways to specify its size:

Fixed length: server sets Content-Length: 12345 and then sends exactly that many bytes.

Chunked transfer encoding: server sets Transfer-Encoding: chunked and sends the body in pieces (chunks) without knowing the total length ahead of time.

The second one is what people mean by a chunked response.
The last chunk has size 0 to signal “done”.

```js
if (req.url === '/sub' && req.method === 'POST') {

    let body = ''

    for await (const chunk of req) { // we can loop over req directly without using something like req.body, as node has done all the work and gives us the chunks directly when we write this.
      body += chunk
    }
  }
```

# sanitization 

“Sanitization is removing anything suspicious from incoming input. In this case, we will be removing any tags from user-uploaded text.”
“An XSS (Cross-Site Scripting) attack is a security vulnerability that allows an attacker to inject malicious scripts into web pages”

install
```
npm install sanitize-html
```

(Only works on strings)

import 
```js
import sanitizeHtml from 'sanitize-html';
```

Use Case
```js
// Example: user-submitted content
const dirty = `<script>alert('xss');</script><p>Hello <b>world</b></p>`;

// Clean it
const clean = sanitizeHtml(dirty);

console.log(clean);
// => "<p>Hello <b>world</b></p>" // also clears styles etc 


sanitizeHtml('h1: <h1>I am in an h1 tag</h1>',  {allowedTags: [], allowedAttributes: {}} ) // no allowed tags and attributes
```

sanitize data for a object :-
```js
import sanitizeHtml from 'sanitize-html' 

export function sanitizeInput(data) {

  const sanitizedData = {}

  for (const [key, value] of Object.entries(data)) {
    if (typeof value === 'string') {
      sanitizedData[key] = sanitizeHtml(value, { allowedTags: ['b'], allowedAttributes: {}})
    } else {
      sanitizedData[key] = value
    }
  }

  return sanitizedData
}
```

# Event 

```js
//syntax

const argument = {
  fullName: 'Meryl Sheep',
  email: 'baah@thedevilwearswool.com',
  phone: 12345678910
}


// import EventEmitter
import { EventEmitter } from 'node:events'

// create the emitter
const emailRequestEmitter = new EventEmitter()
// This is the object that can emit and listen to events.

// define the listener function
function listenerfunction(){}
// This will run when the event fires.


// register the listener
emailRequestEmitter.on('emailRequest', "listenerfunction")
//This says: “Whenever the event named 'emailRequest' happens, call listenerfunction.”


// emit the event
emailRequestEmitter.emit("emailRequest", "argument")
// This triggers all listeners registered for 'emailRequest'.
```

# Server Sent Events (SSE)
It’s a standard way for a server to push data to the browser over HTTP using a long-lived, one-way connection.

Provide a constant stream of data to a client:-
Breaking news
Stock price tickers
Sports scores
Real-time monitoring

Data flows only one way, so not suitable for:-
Chat apps
Doorbell cams

```js
res.statusCode = 200
    res.setHeader('Content-Type', 'text/event-stream')
    res.setHeader('Cache-Control', 'no-cache')
    res.setHeader('Connection', 'keep-alive')


// this will typically be in a interval
res.write(
        `data: ${JSON.stringify({ event: 'temp-updated', temp: temperature})}\n\n` // it signals an end of complete message prompt
      )


// index.js to update the data

const eventSource = new EventSource('/temp/live')

const tempDisplay = document.getElementById('temp-display')

eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data)
  const temperature = data.temp

  tempDisplay.textContent = temperature
}

eventSource.onerror = () => {
  console.log('Connection failed...')
}
```