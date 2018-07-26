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

## Adding pug engine
Install pug
```
npm install pug --save
```
Inside `server.js` set view engine and change route:
```jacascript
app.set('view engine', 'pug')

app.get('/', function (req, res) {
  res.render('index', { title: 'Hey', message: 'Hello there!' })
})
```
Inside `views/` create file `index.pug` and add:
```pug
html
  head
    title= title
  body
    h1= message
```

## Adding Vuetify
Change `index.pug` to:
```pug
doctype html
html
  head
    title= title
    link(href='https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons' rel="stylesheet")
    link(href="https://cdn.jsdelivr.net/npm/vuetify/dist/vuetify.min.css" rel="stylesheet")
    meta(name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, minimal-ui")
  body
    #app
      v-app
        //v-navigation-drawer
        v-toolbar
        v-content
          v-container {{ message }}
        v-footer

    script(src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js")
    script(src="https://cdn.jsdelivr.net/npm/vuetify/dist/vuetify.js")
    script(src="/index.helper.js")
```



    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vuetify/dist/vuetify.js"></script>

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




