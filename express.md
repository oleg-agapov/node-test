# Express.js

Minimal express application:
``` javascript
const http = require('http');
const express = require('express');

const app = express();

const server = http.createServer(app);

server.listen(3000);
```

Express is all about Middleware.
