# res.download
res.download() is an Express.js method used to prompt the client (browser) to download a file. It sends a file as an attachment, meaning the user will get a download dialog instead of displaying the file in the browser.
When you use res.download(), it forces the browser to download the file instead of displaying it in the browser.

```
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

This will not show a console log (if used) on the browser, it will be visible on the terminal of vs code as:-
The reason why console.log isn't showing in your browser's console is because it's running on the server-side, not the client-side.
Server-side logs: The console.log statement inside your Express route logs messages in the terminal where your Node.js server is running.


# res.send
Sends response to the client of various types 
```
app.get('/hello', (req, res) => {
    res.send('Hello, world!');
});
```

# res.end
res.end() is a method in Express.js that signals the end of the response process.
Unlike res.send(), it does not automatically set headers or format the response,it simply terminates the connection.

```
res.end([data]);


app.get('/end', (req, res) => {
    res.write('Processing done.');
    res.end();  // Ends the response
});
```

# res.json
res.json() is an Express.js method used to send a JSON response to the client. It automatically sets the Content-Type header to application/json, making it the preferred way to return structured JSON data.
res.json() helps send data from the server to the client.

```
app.get('/api/user', (req, res) => {
    res.json({ name: 'John Doe', age: 25, role: 'Admin' });
});
```
