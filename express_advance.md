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

##### Environments

##### Configuration

##### Debugging

##### Templating Engines

##### Database Integration

##### Authentication

##### Structuring Express Applications

##### Project- Restructure the App

##### Project- Restructure the App.zip

##### Recap
Express- Advanced Topics Recap.pdf
