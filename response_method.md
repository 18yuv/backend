http is text based protocol, all data transferred bwteen the client and the server must be in string format.
```js
app.get('/', (req, res) => {
  res.json(celebrity)  
})
```

## res.send vs res.json
res.json() is one of the methods on res.
It:
Stringifies the JavaScript object or array you pass into JSON.
Sets the HTTP header Content-Type: application/json.
Ends the response (just like res.send() does).
So basically it’s the easiest way to send JSON data to the client.

How it differs from res.send():-
res.send() can send strings, Buffers, objects, etc.
If you pass an object to res.send(), Express will also JSON-stringify it for you, but it won’t explicitly set Content-Type to application/json in older versions.
res.json() always implies JSON.


Why we use it:-
It’s the standard way to return structured data from an API.
Makes sure the proper headers are set so the client knows it’s JSON.
It automatically handles JSON.stringify for you.


we can only set the headers once every request so we need to carefully write our code so it does that only once or we'll get an error, ex:-

```js
const allowedFields = ['country', 'continent', 'industry']

if (!allowedFields.includes(field)) {
    return res.status(400).json({message: "Search field not allowed. Please use only 'country', 'continent', 'industry'" })
  }

// we are returning the header so that we don't have to face any other error.
```

# res.download
res.download() is an Express.js method used to prompt the client (browser) to download a file. It sends a file as an attachment, meaning the user will get a download dialog instead of displaying the file in the browser.
When you use res.download(), it forces the browser to download the file instead of displaying it in the browser.

```js
res.download(filePath, fileName, callback)

app.get('/download', (req, res) => {
    const filePath = __dirname + '/files/sample.pdf';  // Path to the file
    res.download(filePath, 'DownloadedFile.pdf', (err) => {
        if (err) {
            console.error("Error sending file:", err);
            res.status(500).send("File download failed");
        }
    });
});
```


# res.sendFile
When you use app.put(), you're typically sending data to the server, often for updating existing data.
To send fies to the route like html templates etc:-

```js
app.get('/:slug', (req, res) => {
    res.sendFile('/templates/index.html', { root: __dirname })
})
```

__dirname is a built-in Node.js variable that represents the absolute path of the directory containing the currently executing script.

If your main.js file is inside the projects folder, which itself is inside documents
```js
console.log(__dirname);
// C:\Users\YourName\Documents\Projects

console.log(__filename);
// C:\Users\YourName\Documents\Projects\main.js
```

This will not show a console log (if used) on the browser, it will be visible on the terminal of vs code as:-
The reason why console.log isn't showing in your browser's console is because it's running on the server-side, not the client-side.
Server-side logs: The console.log statement inside your Express route logs messages in the terminal where your Node.js server is running.


# res.send
Sends response to the client of various types 
```js
app.get('/hello', (req, res) => {
    res.send('Hello, world!');
});
```

# res.end
res.end() is a method in Express.js that signals the end of the response process.
Unlike res.send(), it does not automatically set headers or format the response,it simply terminates the connection.

```js
res.end([data]);


app.get('/end', (req, res) => {
    res.write('Processing done.');
    res.end();  // Ends the response
});
```

# res.json
res.json() is an Express.js method used to send a JSON response to the client. It automatically sets the Content-Type header to application/json, making it the preferred way to return structured JSON data.
res.json() helps send data from the server to the client.
    
```js
app.get('/api/user', (req, res) => {
    res.json({ name: 'John Doe', age: 25, role: 'Admin' });
});
```