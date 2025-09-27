# express.Router

Express.Router() is a mini Express application that allows you to create modular, mountable route handlers. It helps organize routes into separate files and makes large applications more manageable.
different pages ke liye diffrent files mein code karo, makes code more manageble and readable.

```js
// involves two main things, routes and controller
//A basic router:-


// server.js
const apiRouter = express.Router()

// controllers
const productsController = (req, res) => {
    res.json({data: 'products'})
}

const servicesController = (req, res) => {
    res.json({data: 'service'})
}

// routes
apiRouter.get('/products', productsController)
// apiRouter.get('/api/products', productsController) // api already mentioned when using

apiRouter.get('/services', servicesController)
// apiRouter.get('/api/services', servicesController)

// use the router
app.use('/api', apiRouter)



// we can further modularise this by making seperate folders for controllers and routes
// error managing when an unknow endpoint is targeted
app.use((req, res) => {
    res.status(404).json({message: 'Endpoint not found'})
})

// this should be at the end of the code like just above the app.listen function as if not all the routes will be handed over to this which we don't want.
```
