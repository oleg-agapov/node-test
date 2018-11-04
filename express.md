# Express.js

Minimal express application:
``` javascript
const http = require('http');
const express = require('express');

const app = express();

const server = http.createServer(app);

server.listen(3000);
```

Express is all about Middleware. Middleware executes before sending a request.
To use middleware use need to register it with:
``` javascript
app.use();
```

Middleware is essentially a function with three parameters – request, responce and a function which should be executed next:
``` javascript
app.use((req, res, next) => {
  // middleware code
});
```
