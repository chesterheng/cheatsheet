#### Express- Advanced Topics

##### Built-In Middleware
* call in sequence
```javascript
app.use(express.json());  // { key: value, key: value }
app.use(express.urlencoded({ extended: true }));  // key=value&key=value
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
