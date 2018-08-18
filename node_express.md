##### Basic Express Setup
* npm init
* edit package.json
```javascript
{
  "name": "devconnector",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

##### Install Dependencies
* dependencies
   * npm i express
   * npm i mongoose (a MongoDB object modeling tool designed to work in an asynchronous environment)
   * npm i body-parser (extract entire body portion of an incoming POST request stream and exposes it on req.body)
   * npm i passport passport-jwt jsonwebtoken
   * npm i bcryptjs validator
* devDependencies
   * npm i -D nodemon (monitor for any changes in source code and automatically restart server)

##### Basic Server Setup
* edit server.js
```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => res.send("Hello!"));

const port = process.env.PORT || 5000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```
* edit package.json
```javascript
{
  "name": "devconnector",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "server": "nodemon server.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "body-parser": "^1.18.3",
    "express": "^4.16.3",
    "jsonwebtoken": "^8.3.0",
    "mongoose": "^5.2.9",
    "passport": "^0.4.0",
    "passport-jwt": "^4.0.0",
    "validator": "^10.6.0"
  },
  "devDependencies": {
    "nodemon": "^1.18.3"
  }
}
```
* npm start (for deployment)
* npm run server (for development)

##### Connecting To MongoDB With Mongoose
* edit dev.js
```javascript
module.exports = {
  mongoURI: "mongodb://<username>:<password>@ds1XXXXX.mlab.com:XXXXX/<projectname>db-dev"
};
```
* edit prod.js
```javascript
module.exports = {
  mongoURI: process.env.MONGO_URI
};
```
* edit keys.js
```javascript
if (process.env.NODE_ENV == "production") {
  // use production keys
  module.exports = require("./prod");
} else {
  // use development keys
  module.exports = require("./dev");
}
```
* edit server.js
```javascript
const express = require("express");
const mongoose = require("mongoose");

const app = express();

// DB config
const db = require("./config/keys").mongoURI;

// Connect to MongoDB
mongoose
  .connect(db)
  .then(() => console.log("MongoDB Connected"))
  .catch(err => console.log(err));

app.get("/", (req, res) => res.send("Hello!"));

const port = process.env.PORT || 5000;

app.listen(port, () => console.log(`Server running on port ${port}`));

```







* install Postman at https://www.getpostman.com/apps
##### Routing methods
* app.get(): read data
* app.post(): insert data
* app.delete(): delete data
* app.patch():
* app.put():

* edit index.html
```html
<!DOCTYPE html>
<html lang="en">
   <body>
      <form action="/name" method="post">
         <label>First Name :</label>
         <input type="text" name="first" value="John"><br>
         <label>Last Name :</label>
         <input type="text" name="last" value="Doe"><br><br>
         <input type="submit" value="Submit">
      </form>
   </body>
</html>
```
* edit index.js
```javascript
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

/** bodyParser.urlencoded(options)
 * Parses the text as URL encoded data (which is how browsers tend to send form data from regular forms set to POST)
 * and exposes the resulting object (containing the keys and values) on req.body
 */
app.use(bodyParser.urlencoded({
    extended: true
}));

/**bodyParser.json(options)
 * Parses the text as JSON and exposes the resulting object on req.body.
 */
app.use(bodyParser.json());

app.get('/', (req, res) => {
   // Send a JSON response
   res.json({ hi: 'there' });
});

// get data from form POST
// req.body = { first: 'John', last: 'Doe' }
app.post('/name', (req, res) => {
   const firstname = req.body.first;
   const lastname = req.body.last;
   const name = `${firstname} ${lastname}`;
   res.json({ name: name });
});

// all other routes
app.all('*', (req, res) => {
   // Send a response of a String, an object, or an Array   
   res.send("Not Found.");
});

app.listen(5000);
```

#### Introduction to the Basic Node and Express Challenges
* edit myApp.js
```javascript
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const app = express();

// --> 7)  Mount the Logger middleware here
const myLogger = (req, res, next) => {
  console.log(`${req.method} ${req.path} - ${req.ip}`); 
  next();
};
app.use(myLogger);

// --> 11)  Mount the body-parser middleware here
app.use(bodyParser.urlencoded({     // to support URL-encoded bodies
  extended: false
})); 

/** 1) Meet the node console. */
console.log("Hello World");

/** 2) A first working Express Server */
/*app.get("/", (req, res) => {
  res.send("Hello Express");
});*/

/** 3) Serve an HTML file */
app.get("/", (req, res) => {
  const absolutePath = path.join(__dirname, "/views/index.html");
  res.sendFile(absolutePath);
});

/** 4) Serve static assets  */
const publicPath = path.join(__dirname, "/public");
app.use("/", express.static(publicPath));

/** 5) serve JSON on a specific route */
/*app.get("/json", (req, res) => {
  res.json({"message": "Hello json"});
});*/

/** 6) Use the .env file to configure the app */
app.get("/json", (req, res) => {
  let data = {"message": "Hello json"};
  if(process.env.MESSAGE_STYLE === 'uppercase'){
    data.message = data.message.toUpperCase();
  }
  res.json(data);
});
 
/** 7) Root-level Middleware - A logger */
//  place it before all the routes !


/** 8) Chaining middleware. A Time server */
const requestTime = (req, res, next) => {
  req.time = new Date().toString();
  next();
};

app.get("/now", requestTime, (req, res) => {
  res.send({time: req.time});
});

/** 9)  Get input from client - Route parameters */
app.get("/:word/echo", (req, res) => res.json({echo: req.params.word}));

/** 10) Get input from client - Query parameters */
// /name?first=<firstname>&last=<lastname>
app.route('/name').get((req, res) => {
  let firstname = req.query.first;
  let lastname = req.query.last;
  let name = `${firstname} ${lastname}`;
  res.send({ name: name });
}).post();
  
/** 11) Get ready for POST Requests - the `body-parser` */
// place it before all the routes !

/** 12) Get data form POST  */
app.post('/name', (req, res) => {
  let firstname = req.body.first;
  let lastname = req.body.last;
  let name = `${firstname} ${lastname}`;
  res.json({ name: name });
});


// This would be part of the basic setup of an Express app
// but to allow FCC to run tests, the server is already active
/** app.listen(process.env.PORT || 3000 ); */

//---------- DO NOT EDIT BELOW THIS LINE --------------------

 module.exports = app;
```
