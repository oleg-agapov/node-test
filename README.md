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
