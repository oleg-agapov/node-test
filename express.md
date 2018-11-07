# Express.js
Install Express to the current project with:
``` bash
npm install --save express
```
Minimal express application:
``` javascript
const express = require('express');

const app = express();

app.listen(3000);
```

Express is all about Middleware. Middleware executes before sending a request.
To use middleware use need to register it with:
``` javascript
app.use();
```
### Middleware
Middleware is essentially a function with three parameters – `request`, `responce` and a function which tells that express should run next middleware in line, e.g.:
``` javascript
app.use((req, res, next) => {
  console.log('In the first middleware');
  next(); // <-- this code will run the next middleware in chain
});


app.use((req, res, next) => {
  console.log('In the second middleware');
});
```
If there is no need to call another middleware you should return responce. Express doesn't send any responce for you. You can send it with the next command:
``` javascript
res.send();
```
Inside send you can put:
* html code. In this case express will set header `Content-Type` for you as `text/html`
* alternatively, most of other data will be returned as `json`

### Routing
To add anew route you can use the following syntax:
``` javascript
app.use('/users', (req, res, next) => {
  // callback function code
});

app.use('/', anotherCallbackFunction);
```
Here, "/" is a kind of a wildcard, meaning all routes matche it. So the order matters here. Also, if you will call next() finction in the first call instead of sending request you will fall back to "/" route callback.

### Redirecting
To perform a redirect simply send the following responce:
``` javascript
res.redirect('/');
```

### Parsing requests
To access to the incoming requests data you need to do two things:
1. to add a parser middleware
2. use `res.body` to access the parsed data
Install a new package middeleware:
``` bash
npm install --save body-parser
```
This parser is used for parsing data from forms. To use it you need to import it first and then register it as a middleware:
``` javascript
const bodyParser = require('body-parser');

// you need to execute urlencoded() method of bodyParser
app.use(bodyParser.urlencoded({extended: false}));
```
### HTTP methods
If you need to handle a specific HTTP method (like GET, POST, DELETE, PUT etc.) you can use the following syntax:
``` javascript
app.get(...); // will handle only GET requests
app.post(...); // will handle only POST requests

app.use(...); // will handle all HTTP methods
```

### Express router
To split the application into smaller parts we can utilize Express router.
Create a folder /router and add a new file `home.js`. Put the following code there:
``` javascript
const express = require('express');

const router = express.Router();

router.get('/', callbackFunction);

module.exports = router;
```
This code creates a router and add '/' route handler. Now you can import this router to main application file and use it as a middleware:
``` javascript
const homeRouter = require('./routes/home');

app.use(homeRouter);
```
If you want to redirect user to another page just run:
``` javascript
res.redirect('/');
```

### Filtering routes
To handle only specific routes with specific handler you can use filters when registering a router like this:
``` javascript
app.use('/admin', adminRoutes);
```
Handler `adminRoutes` will be executed only for routes which start with `/admin/`.
### 404 page
To have 404 page just add the following code after registering all routing middlewares:
``` javascript
app.use((req, res, next) => {
  res.status(404).send('Page not found');
});
```
### Serving HTML files
Create a HTML file in `/views` folder (e.g. `index.html`). To render this file as a responce send it from the router like this:
``` javascript
const path = require('path');

// in middleware
res.sendFile(path.join(__dirname, '..', 'views', 'index.html'));
```
We can to send HTML with `.sendFile()` method. We need to use `path` module to correctly resolve path to the file in different operating systems. Additionaly we need to use `'..'` because we store router in a subfolder to `app.js`. If you call `sendFile` from a root folder you can omit `'..'`.
Instead of using `__dirname` and `'..'` we can create a helper function. Create path.js file and put there the following:
``` javascript
const path = require('path');

modules.exports = path.dirname(process.mainModule.filename);
```
This helper constant will always point to main execution file of our server (e.g. app.js). To use it, you need to import it and replace `__dirname` with it:
``` javascript
const rootDir = require('./path');

// in middleware
res.sendFile(path.join(rootDir, 'views', 'index.html'));
```
### Styling and serving static files
To serve static content (such as CSS styling) you need to make it expose it first. Create a folder `/public`. Put your styling, images or JavaScript files there. Then you need to grant access to this folder, in `app.js` add middleware:
``` javascript
const path = require('path');

app.use(express.static(path.join(__dirname, '/public')));
```
### Adding template engine Pug(Jade)
Install Pug into your project:
``` bash
npm install --save pug
```
Then add support of the template engine in `app.js` file with:
``` javascript
app.set('view engine', 'pug'); // <-- this sets a template engine
app.set('views' , 'views'); // <-- second constant is a folder with templates
```
Finally, to render your template use the following syntax:
``` javascript
res.render('index');
```




