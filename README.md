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

Add commands to `package.json` to start up the server:
```json
"scripts": {
    "start": "nodemon src/app.js --exec  \"npm run lint && node\"",
    "lint": "eslint src/**/*.js",
    "lint:fix": "eslint src/**/*.js  --fix"
}
```
