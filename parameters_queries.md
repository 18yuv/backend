
# Parameter and Queries

Parameters are values sent in the URL that allow dynamic routing and data handling.
There are two main types:-


route parameters :- Defined using a : before a parameter name in the route.
                    used by req.params. 
query parameters :- Passed as key-value pairs in the URL after a ?.
                    Extracted using req.query.
                    this is extra data in the url , /search?q=  


route :- A route defines a specific URL endpoint and the logic that executes when that endpoint is accessed, A route is a specific path in the server that defines what happens when a request is made to that path.


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
