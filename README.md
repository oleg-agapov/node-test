# node-test

Node.js & NPM installation -> https://nodejs.org/en/


## NPM initialization
In project folder run
```bash
npm init -f
```
Change `main` value from `index.js` to `server.js`.

## Express.js

Install express.js:
```
npm install --save express
```
"Hello World" application, create `server.js` and add:
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
node server.js
```

## nodemon, commands to package.json

> nodemon is a tool that helps develop node.js based applications by automatically restarting the node application when file changes in the directory are detected
```
npm install --save-dev nodemon
```
Add commands to `package.json` to start up the server with auto updates:
```
"scripts": {
    "dev": "nodemon"
}
```
Now run server with
```
npm run dev
```
**! This will work only for back-end code (Node.js), dont's expect updating of HTML in browser !**

## Environment variables
> Dotenv is a zero-dependency module that loads environment variables from a `.env` file into `process.env`. 
Create `.env` file:
```
PORT=3000
```
Install dotevn
```
npm install dotenv
```
Load variables into application using `dotenv`:
```javascript
require('dotenv').config()
```

## d
## r
## a
## f
## t


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

NODE_ENV=development




