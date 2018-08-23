#### Express- Advanced Topics

##### Built-In Middleware
* call in sequence
```javascript
// Postman Header -> raw: JSON(application/json)
// Postman Body -> { "name": "My course" }
app.use(express.json());  // { key: value, key: value }

// Postman Header -> x-www-form-urlencoded
// Postman Body -> Key: name, Value: My course
app.use(express.urlencoded({ extended: true }));  // key=value&key=value

// edit public\readme.txt
// localhost:3000/readme.txt
app.use(express.static('public'));  // server static files
```

##### Creating Custom Middleware
* edit middleware\logger.js
```javascript
const logger = (req, res, next) => {
  console.log('Logging...');
  next();
};

module.exports = logger;
```javascript

* edit server.js
```javascript
const logger = require('./middleware/logger');

app.use(express.json());
app.use(logger);
```javascript

##### Third-party Middleware
* https://expressjs.com/en/resources/middleware.html
* npm i helmet (Helps secure your apps by setting various HTTP headers)
* npm i morgan (HTTP request logger)
```javascript
const Joi = require('joi');
const express = require('express');
const helmet = require('helmet');
const morgan = require('morgan');
const logger = require('./middleware/logger');

const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static('public'));
app.use(helmet());
app.use(morgan('tiny'));
app.use(logger);
```

##### Environments
```javascript
// window: SET NODE_ENV=development
// OS X: export NODE_ENV=development
if (process.env.NODE_ENV === 'development') {
  app.use(morgan('tiny'));
  debug('Morgan enabled...');
}

if (app.get('env') === 'development') {
  app.use(morgan('tiny'));
  debug('Morgan enabled...');
}
```

##### Configuration
* https://codingsans.com/blog/node-config-best-practices
* npm i dotenv 
* edit .env
```
PORT=3000
```
* edit server.js
```javascript
require('dotenv').config();
const port = process.env.PORT;
```

##### Debugging
* npm i debug

* edit .env
```
DEBUG=app:startup
DEBUG=app:startup,app:db
DEBUG=app:*
DEBUG=app:startup nodemon server.js
```
* edit server.js
```javascript
const startupDebugger = require('debug')('app:startup');
const dbDebugger = require('debug')('app:db');
startupDebugger('Morgan enabled...');
```

##### Templating Engines
* npm i pug
* edit views\index.pug
```
html
  head
    title= title
  body
    h1= message
```
* edit server.js
```javascript
const app = express();

app.set('view engine', 'pug');
app.set('views', './views');

app.get('/', (req, res) => {
  res.render('index', { title: 'My Express App', message: 'Hello' });
});
```

##### Database Integration

##### Authentication

##### Structuring Express Applications

##### Project- Restructure the App

##### Project- Restructure the App.zip

##### Recap
Express- Advanced Topics Recap.pdf
