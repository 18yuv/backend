# Parameter and Queries
Parameters are values sent in the URL that allow dynamic routing and data handling.
There are two main types:-

route parameters :- 
Defined using a : before a parameter name in the route.
used by req.params.

query parameters :- 
Passed as key-value pairs in the URL after a ?.
Extracted using req.query.
this is extra data in the url , /search?q=  
queries (?) are used basically to share small data with the database formode=dark&region=in


route :- A route defines a specific URL endpoint and the logic that executes when that endpoint is accessed, A route is a specific path in the server that defines what happens when a request is made to that path.


```js
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

```js
// for url : http://localhost:8000/kuch/bhi?darkmode=true

app.get('/:page/:slug', (req, res) => {
    console.log(req.params); // consoles the params object
    console.log(req.query); // consoles the query object
    console.log(`<h1>Hello! ${req.params.page} and ${req.params.slug}</h1>`)
    res.end();
})


/* output:-
[Object: null prototype] { page: 'kuch', slug: 'bhi' }
[Object: null prototype] { darkmode: 'true' }
<h1>Hello! kuch and bhi</h1>
*/

// to convert the returned boolean which is a string into a boolean again we can use, json.parse
const sbool = "true";
const parsed = JSON.parse(sbool)
console.log(typeof(sbool)); // string
console.log(typeof(parsed)); // boolean

// to again reverse this process we can use jsono.stringify()
// It takes a JavaScript value (object, array, boolean, number, etc.) and converts it into a JSON-formatted string.


// we can use the req.query by destructuring it, by the values that we expect then further filter them
const { industry, country, continent, is_seeking_funding, has_mvp } = req.query

if (industry) {
  filteredData = filteredData.filter( startup => 
    startup.industry.toLowerCase() === industry.toLowerCase()
  )
}

if (country) {
  filteredData = filteredData.filter( startup => 
    startup.country.toLowerCase() === country.toLowerCase()
  )
}
```
