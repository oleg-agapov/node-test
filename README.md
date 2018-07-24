# node-test

Setting up Node.js
```bash
npm init -f
```

Setting up hot reload (`nodemon`) and linting package (`eslint`)
```
npm install --save nodemon eslint
```

Initializing linting
```
node ./node_modules/eslint/bin/eslint.js --init
```

Setting up main packages:
* `express` - Express framework
* `body-parser` - parsing incoming data from forms and JSON
* `cors` - enabling CORS
* `morgan` - logging

```
npm install --save express body-parser cors morgan
```

"Hello World" application:
```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    res.send('Node-express is here!')
})

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

Add commands to `package.json` to start up the server (first two; others are just for example purposes):
```json
"scripts": {
    "start": "nodemon src/app.js --exec  \"npm run lint && node\"",
    "lint": "eslint src/**/*.js",
    "lint:fix": "eslint src/**/*.js  --fix",
    "prod": "node ./start.js",
    "watch": "nodemon ./start.js --ignore public/",
    "start": "concurrently \"npm run watch\" \"npm run assets\" --names \"💻,📦\" --prefix name",
    "assets": "webpack -w --display-max-modules 0",
    "sample": "node ./data/load-sample-data.js",
    "blowitallaway": "node ./data/load-sample-data.js --delete"
"now": "now -e DB_USER=@db_user -e DB_PASS=@db_pass -e NODE_ENV=\"production\" -e PORT=80"
}
```

Create `variables.env` file:
```
NODE_ENV=development
PORT=8081
```

Load variables into application using `dotenv`:
```javascript
require('dotenv').config({ path: 'variables.env' });
```


