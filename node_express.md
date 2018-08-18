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

##### Route Files With Express Router
* edit routes/api/users.js
```javascript
const express = require("express");
const router = express.Router();

// @route   GET api/users/test
// @desc    Test users route
// @access  Public
router.get("/test", (req, res) => res.json({ msg: "Users Works" }));

module.exports = router;
```
* edit server.js
```javascript
const express = require("express");
const mongoose = require("mongoose");
const keys = require("./config/keys");
const users = require("./routes/api/users");
const app = express();

// Connect to MongoDB
mongoose
  .connect(keys.mongoURI)
  .then(() => console.log("MongoDB Connected"))
  .catch(err => console.log(err));

app.get("/", (req, res) => res.send("Hello!"));

// Use Routes Middleware
app.use("/api/users", users);

const port = process.env.PORT || 5000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```
* Postman: https://www.getpostman.com/
* test: http://localhost:5000/api/users/test
* git init
* git add .
* git commit -am 'Initial express server with route files'

##### Add body parser middleware
* edit server.js
```javascript
const bodyParser = require("body-parser");

// Body parser middleware
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

// Connect to MongoDB
mongoose
  .connect(keys.mongoURI)
  .then(() => console.log("MongoDB Connected"))
  .catch(err => console.log(err));

app.get("/", (req, res) => res.send("Hello!"));

// Use Routes
app.use("/api/users", users);
app.use("/api/profile", profile);
app.use("/api/posts", posts);

const port = process.env.PORT || 5000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```
