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
To pass a parameters to you template simply add a dictionary with data to `render` function:
``` javascript
res.render('index', {data: data});
```
### Dynamic routes and parameters
To create dynamic route you need to add it to router like this:
``` javascript
router.get('/products/:productId', controller);
```
Now in controller you can extract `productId` parameter from request object:
``` javascript
const productId = req.params.productId
```
If you want to access query parameters `?search=keyword` from URL you can use:
``` javascript
const searchParam = req.query.search
```
## Databases
### Adding MySQL to data models
First install package with connection drivers for MySQL:
``` bash
npm install --save mysql2
```
Create a connection object in `/helpers` folder in `database.js`:
``` javascript
const mysql = require('mysql2');

const pool = mysql.CreatePool({
  host: <HOSTNAME>,
  user: <USERNAME>,
  database: <DB_NAME>,
  password: <PASSWORD>
});

module.exports = pool.promise();
```
This code creates a pool of connections we can use in application. Also we use promise-based connection meaning you will get an answer in Promise. Now add support of DB in models:
``` javascript
const db = require('../helpers/database');

module.exports = class Users {
  constructor(id, email, name) {
    this.id = id;
    this.email = email;
    this.name = name;
  }

  save() {
    return db.execute(
      'INSERT INTO users (email, name) VALUES (?, ?)',
      [this.email, this.name]
    );
  }
  
  static fetchAll() {
    return db.execute('SELECT * FROM users');
  }

  static getById(id) {
    return db.execute('SELECT * FROM users WHERE users.id = ?', [id]);
  }
};
```
Finally, to get results in controller use Promise-based `.then().catch()` syntax to get the result os the query:
``` javascript
// saving user
user.save()
  .then(() => {
    // successfuly saved new user
  })
  .catch(err => {
    // handle error `err` here
  });
```
### Adding MongoDB through Mongoose
Install Mongoose package:
``` bash
npm install --save mongoose
```
To connect to MongoDB use connect method in `app.js`:
``` javascript
mongoose
  .connect(<MONGO_DB_CONNECTION_STRING>)
  .then(result => {
    // at this stage we are connected to DB
    app.listen(3000);
  })
```
Now we can define data model. First of all define schema - key-value dictionary. Specify type of each field  (standard JavaScript data types) and any additional parameters from documentation. If you need to define custom methods create it under `schema.methods`. You need to use usual function syntax (not arrow function) in order to have access to schema through `this` keyword. Finally, export model of your data by passing model name and schema. An example of `User` model:
``` javascript
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const userSchema = new Schema({
  email: {
    type: String,
    required: true
  },
  password: {
    type: String,
    required: true
  },
  age: Number
});

userSchema.methods.customMethod = function() {
  //...
};

module.exports = mongoose.model('User', userSchema);
```
To use this model in your controller you need simply import it, basic CRUD operations will be awailable out of the box. Almost every operation will return a Promise so you can chain it with `then().catch()` block:
``` javascript
// retrieve all users
User.find()
  .then(users => {
    console.log(users);
  })
  .catch(error => {
    console.log(error);
  });

// find user by id
User.findById(userId);

// find user by name
User.find({ 'name': 'Max' })

// save user to DB
const user = new User({ name: 'Max', email: 'max@test.com' });
user.save()

// find by id and update name
User.findById(userId)
  .then(user => {
    user.name = 'Maximilian';
    return user.save()
  })
  .then(updatedUser => {})

// find by id and remove
User.findByIdAndRemove(userId)
```
#### Adding reference field
If you want to add a field which is a reference to another document in other collection you can define it like this:
``` javascript
const productSchema = new Schema({
  ...
  userId: {
    type: Schema.Types.ObjectId,
    ref: 'User',
    required: true
  }
});
```
In the example above `userId` field of `Product` model is a reference to `_id` field in `User` model.
If you want to retrieve referenced `User` object during quering `Product` model you can chain `populate` method:
``` javascript
Product.findById(productId).populate('userId');
```
This method will extract corresponding user's document and attach it to `userId` field of the result.
If you already have extracted product but you want to rerun the query to populate user you can do it like this:
``` javascript
const product = Product.findById(productId);

product.populate('userId').execPopulate();
```
If you need to retrieve only specific fields you can use `select` method:
``` javascript
User.findOne().select('name email');
```
### Sessions
Install session helper for Express:
``` bash
npm install --save express-session
```
To store the sessions not in memory but persist them in MongoDB install additionally:
``` bash
npm install --save connect-mongodb-session
```
Now you need to add sessions functionality as a middleware in `app.js`:

``` javascript
const session = require('express-session');
const MongoDBStore = require('connect-mongo-session')(session);

const store = new MongoDBStore({
  uri: <MONGO_DB_URI>,
  collection: 'sessions'
});

app.use(
  session({
    secret: <SECRET_KEY>,
    resave: false,
    saveUninitialized: false,
    store: store
  })
);
```
To create and read a session values use `req.session` object in controller:
``` javascript
req.session.user = { name: 'Max' }  // creating of user object
console.log(req.session)            // outputing session content to console
```
To delete a session use `destroy` method:
``` javascript
req.session.destroy((err) => {
  res.redirect('/');
});
```
If you want to get a callback after your session was saved successfully (for example if you need to make sure that you render your view _after_ the session was deleted) use:
``` javascript
req.session.save((err) => {
  // callback is here
});
```
### Handling cookies
Install cookie parser package:
``` bash
npm install --save cookie-parser
```
Add it as a middleware:
``` javascript
var cookieParser = require('cookie-parser');

app.use(cookieParser());
```
To set a cookie use `res.cookie` object:
```
res.cookie('name', 'Max', { expires: new Date(Date.now() + 900000), httpOnly: true });
```
To read a cookie use `req.cookies` object:
``` javascript
req.cookies.name  // => "Max"
```
## Authentication 
### Prerequisites
First you should have a `User` model:
``` javascript
onst mongoose = require('mongoose');

const Schema = mongoose.Schema;

const userSchema = new Schema({
  email: {
    type: String,
    required: true
  },
  password: {
    type: String,
    required: true
  }
});

module.exports = mongoose.model('User', userSchema);
```

### Signup
Create a controller to handle submitting of new user credentials:
``` javascript
const signupUser = (req, res, next) => {

};
```
### Login


