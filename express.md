# Express.js

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
  console.log("In the first middleware");
  next(); // <-- this code will run the next middleware in chain
});


app.use((req, res, next) => {
  console.log("In the second middleware");
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
app.use("/users", (req, res, next) => {
  // callback function code
});

app.use("/", anotherCallbackFunction);
```
Here, "/" is a kind of a wildcard, meaning all routes matche it. So the order matters here. Also, if you will call next() finction in the first call instead of sending request you will fall back to "/" route callback.




