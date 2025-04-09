# Express js

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

# hello world

```
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

# iParameter and Queries

parameters are values sent in the URL that allow dynamic routing and data handling. There are two main types:
route parameters :- Defined using a : before a parameter name in the route.
                    used by req.params. 
query parameters :- Passed as key-value pairs in the URL after a ?.
                    Extracted using req.query.
                    this is extra data in the url , /search?q=  

```
app.get('/', (req, res) => {
  res.send('<h1>Hello!</h1>')
})
app.get('/about', (req, res) => {
  res.send('<h1>Hello! about</h1>')
})
app.get('/contact', (req, res) => {
  res.send('<h1>Hello! contact</h1>')
})
app.get('/yuvi', (req, res) => {
  res.send('<h1>Hello! yuvi</h1>')
})
```

A faster way to achieve these end points using express could be:-

```
app.get('/:page/:slug', (req, res) => {
    console.log(req.params); // consoles the params object
    console.log(req.query); // consoles the query object
  res.send(`<h1>Hello! ${req.params.page} and ${req.params.slug}</h1>`)
})
```

# express.static

To access public files, create a folder (public named folder is the standard) and list the files there then:-

```
app.use(express.static('public'))
```