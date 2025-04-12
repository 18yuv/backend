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

# express.static

To access public files, create a folder (public named folder is the standard) and list the files there then:-

```
app.use(express.static('public'))
```

# Post req

we can use the traditional way of testing our post reqs for testing api's by making an html page.

```
app.post('/', (req,res)=>{
    res.send('<h1>Its a post req</h1>')
})

<script>
        (async function testPost() {
            let a = await fetch("/", {method : "POST"});
            let b = await a.text();
            console.log(b);
        })();
</script>
```

# res.sendFile

To send fies to the route like html templates etc:-

```
app.get('/:slug', (req, res) => {
    res.sendFile('/templates/index.html', { root: __dirname })
})
```

__dirname is a built-in Node.js variable that represents the absolute path of the directory containing the currently executing script.

If your main.js file is inside the projects folder, which itself is inside documents
```
console.log(__dirname);
// C:\Users\YourName\Documents\Projects

console.log(__filename);
// C:\Users\YourName\Documents\Projects\main.js
```