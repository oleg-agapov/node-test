# node-test

Node.js & NPM installation -> https://nodejs.org/en/


Setting up Node.js
```bash
npm init -f
```

### Express.js

Install express.js:
```
npm install --save express
```
"Hello World" application, create `src/index.js` and add:
```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    res.send('Express.js application is here!')
})

app.listen(3000, () => console.log('Express.js app listening on port 3000!'))
```
To see result run
```
node index.js
```

### nodemon, commands to package.json

Setting up hot reload (`nodemon`) module
```
npm install --save nodemon
```
Add commands to `package.json` to start up the server (first two; others are just for example purposes):
```
"scripts": {
    "dev": "nodemon src/app.js --exec  \"node\""
}
```

#### Environment variables

Create `variables.env` file:
```
NODE_ENV=development
PORT=8081
```

Load variables into application using `dotenv`:
```javascript
require('dotenv').config({ path: 'variables.env' });
```

## #############


linting package (`eslint`)
```
npm install --save eslint
```
Initializing linting
```
node ./node_modules/eslint/bin/eslint.js --init
```


Setting up main packages:
* `body-parser` - parsing incoming data from forms and JSON
* `cors` - enabling CORS
* `morgan` - logging in console
```
npm install --save express body-parser cors morgan
```






