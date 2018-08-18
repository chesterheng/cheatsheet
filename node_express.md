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
   * npm i [express](https://expressjs.com/)
   * npm i [mongoose](https://mongoosejs.com/) (mongodb object modeling)
   * npm i [body-parser](https://www.npmjs.com/package/body-parser) (extract entire body portion of an incoming POST request stream and exposes it on req.body)
   * npm i passport passport-jwt jsonwebtoken
   * npm i [bcryptjs](https://www.npmjs.com/package/bcryptjs) (hash password)
   * npm i validator
* devDependencies
   * npm i -D nodemon (monitor for any changes in source code and automatically restart server)

##### Basic Server Setup
* edit server.js
```javascript
const express = require('express');
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
* edit server.js
```javascript
const express = require('express');
const mongoose = require('mongoose');
const keys = require('./config/keys');
const users = require('./routes/api/users');
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
* edit routes/api/users.js
```javascript
const express = require('express');
const router = express.Router();

// @route   GET api/users/test
// @desc    Test users route
// @access  Public
router.get("/test", (req, res) => res.json({ msg: "Users Works" }));

module.exports = router;
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

##### POST request User Registration & Postman
* npm i [gravatar](https://github.com/emerleite/node-gravatar) (generate gravatar URLs)
* edit routes/api/users.js
```javascript
const express = require("express");
const router = express.Router();
const User = require("../../models/User");
const gravatar = require("gravatar");
const bcrypt = require("bcryptjs");

// @route   POST api/users/register
// @desc    Register user
// @access  Public
router.post("/register", async (req, res) => {
  // check existing user
  const user = await User.findOne({ email: req.body.email });
  if (user) return res.status(400).json({ email: "Email aready exists" });

  // get user avatar url
  // eg. "//www.gravatar.com/avatar/86a350637982265bd40f45fbaad41667?s=200&r=pg&d=mm"
  const avatar = gravatar.url(
    req.body.email,
    { s: "200", r: "pg", d: "mm" },
    true
  );

  // create new user
  const newUser = new User({
    name: req.body.name,
    email: req.body.email,
    avatar,
    password: req.body.password
  });

  // hash new user's password
  bcrypt.genSalt(10, (err, salt) => {
    bcrypt.hash(newUser.password, salt, async (err, hash) => {
      if (err) throw err;
      newUser.password = hash;
      // save new user
      await newUser.save((err, user) => {
        if (err) console.log(err);
        res.json(user);
      });
    });
  });
});

module.exports = router;
```
* test with Postman
  * method: POST
  * Content-Type: x-www-form-urlencoded
  * KEY: name, VALUE: zzzzzzzz
  * KEY: email, VALUE: yyyyyyyy@gmail.com
  * KEY: password, VALUE: xxxxxxxx
  * POST request generated  
```
POST /api/users/register HTTP/1.1
Host: localhost:5000
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache
Postman-Token: 85b8c30c-c577-4bb1-a39c-4fa60a03ec78

name=zzzzzzzz&email=yyyyyyyy%40gmail.com&password=xxxxxxxx
```
  * new user created
```
{
    "_id": "5b77a5a47d076723d0e894df",
    "name": "zzzzzzzz",
    "email": "yyyyyyyy@gmail.com",
    "avatar": "https://s.gravatar.com/avatar/86a350637982265bd40f45fbaad41667?s=200&r=pg&d=mm",
    "password": "$2a$10$lz9subWSymqWZe5ARNMN1uhvPe/biIkrhpJ9uerUxv56DeJSrQfhi",
    "date": "2018-08-18T04:50:44.708Z",
    "__v": 0
}
```
##### Email & Password Login
* edit routes/api/users.js
```javascript
const express = require("express");
const router = express.Router();
const User = require("../../models/User");
const gravatar = require("gravatar");
const bcrypt = require("bcryptjs");

// @route   GET api/users/login
// @desc    Login User / Returning Token
// @access  Public
router.post("/login", async (req, res) => {
  const email = req.body.email;
  const password = req.body.password;

  // find user by email
  const user = await User.findOne({ email });
  // check for user
  if (!user) return res.status(404).json({ email: "User not found" });

  // check password
  const isMatch = await bcrypt.compare(password, user.password);
  if (isMatch) return res.json({ msg: "Success" });
  return res.status(400).json({ password: "Password incorrect" });
});

module.exports = router;
```
* test with Postman
  * method: POST
  * Content-Type: x-www-form-urlencoded
  * KEY: email, VALUE: yyyyyyyy@gmail.com
  * KEY: password, VALUE: xxxxxxxx
  * POST request generated  
  
##### Creating The JWT

