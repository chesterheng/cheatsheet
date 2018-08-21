##### Best practices for Express app structure
* controllers/ – defines your app routes and their logic
* helpers/ – code and functionality to be shared by different parts of the project
* middlewares/ – Express middlewares which process the incoming requests before handling them down to the routes
* models/ – represents data, implements business logic and handles storage
* public/ – contains all static files like images, styles and javascript
* views/ – provides templates which are rendered and served by your routes
* tests/ – tests everything which is in the other folders
* server.js – initializes the app and glues everything together
* package.json – remembers all packages that your app depends on and their versions

##### Node/CommonJS modules
* keys.js
```javascript
module.exports = {
  mongoURI: 'mongodb://<username>:<password>@ds125402.mlab.com:25402/<dbname>'
};
```
* keys.js
```javascript
if (process.env.NODE_ENV == 'production') {
  // use production keys
  module.exports = require('./prod');
} else {
  // use development keys
  module.exports = require('./dev');
}
```
* edit server.js
```javascript
const keys = require('./config/keys');
```

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

app.get('/', (req, res) => res.send('Hello!'));

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

##### Request and Response Objects
* Request Object
  * req.body: an object contains POST parameters
  * req.params: an object contains named route parameters
  * req.user: contains the authenticated user (need passport)
* Response Object
  * res.json(json): sends JSON to client with an optional status code
  * res.json(status, json): sends JSON to client with an optional status code
 
##### Route Files With Express Router
* edit server.js
```javascript
// Module dependencies
const express = require('express');
const mongoose = require('mongoose');

const keys = require('./config/keys');
const users = require('./routes/api/users');

// Create Express server.
const app = express();

// Connect to MongoDB
mongoose
  .connect(keys.mongoURI)
  .then(() => console.log('MongoDB Connected'))
  .catch(err => console.log(err));

// Primary app routes
app.get("/", (req, res) => res.send("Hello!"));

// Use Routes Middleware
app.use('/api/users', users);

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
router.get('/test', (req, res) => res.json({ msg: 'Users Works' }));

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
const bodyParser = require('body-parser');

// Connect to MongoDB
mongoose
  .connect(keys.mongoURI)
  .then(() => console.log('MongoDB Connected'))
  .catch(err => console.log(err));
  
// Body parser middleware
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.get("/", (req, res) => res.send("Hello!"));

// Use Routes Middleware
app.use('/api/users', users);

const port = process.env.PORT || 5000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```
##### Use HTTP Methods & API Routes
* POST /user or PUT /user:/id to create a new user,
* GET /user to retrieve a list of users,
* GET /user/:id to retrieve a user,
* PATCH /user/:id to modify an existing user record,
* DELETE /user/:id to remove a user.

##### Use HTTP Status Codes Correctly
* 2xx, if everything was okay,
* 3xx, if the resource was moved,
* 4xx, if the request cannot be fulfilled because of a client error (like requesting a resource that does not exist),
* 5xx, if something went wrong on the API side (like an exception happened).
* Example: 
  * res.status(400).json({ email: 'Email aready exists' });
  * res.status(400).json({ password: 'Password incorrect' });
  * res.status(404).json({ email: 'User not found' });
  * res.status(500).json({error: 'Internal server error happened'});

##### POST request User Registration & Postman
* npm i [gravatar](https://github.com/emerleite/node-gravatar) (generate gravatar URLs)
* edit routes/api/users.js
```javascript
const express = require('express');
const router = express.Router();
const User = require('../../models/User');
const gravatar = require('gravatar');
const bcrypt = require('bcryptjs');

// @route   POST api/users/register
// @desc    Register user
// @access  Public
router.post('/register', async (req, res) => {
  // check existing user
  const user = await User.findOne({ email: req.body.email });
  if (user) return res.status(400).json({ email: 'Email aready exists' });

  // get user avatar url
  // eg. "//www.gravatar.com/avatar/86a350637982265bd40f45fbaad41667?s=200&r=pg&d=mm"
  const avatar = gravatar.url(
    req.body.email,
    { s: '200', r: 'pg', d: 'mm' },
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
const express = require('express');
const router = express.Router();
const User = require('../../models/User');
const gravatar = require('gravatar');
const bcrypt = require('bcryptjs');

// @route   POST api/users/login
// @desc    Login User / Returning Token
// @access  Public
router.post('/login', async (req, res) => {
  const email = req.body.email;
  const password = req.body.password;

  // find user by email
  const user = await User.findOne({ email });

  // user not found
  if (!user) return res.status(404).json({ email: 'User not found' });

  // user found, check password
  const isMatch = await bcrypt.compare(password, user.password);

  // wrong password
  if (!isMatch) return res.status(400).json({ password: 'Password incorrect' });

  // correct password
  return res.json({ msg: 'Success' });
});

module.exports = router;
```
* test with Postman
  * method: POST
  * Content-Type: x-www-form-urlencoded
  * KEY: email, VALUE: yyyyyyyy@gmail.com
  * KEY: password, VALUE: xxxxxxxx
  * POST request generated  
  
##### Creating The JWT-Based, Stateless Authentication
* edit routes/api/users.js
```javascript
const express = require('express');
const router = express.Router();
const User = require('../../models/User');
const gravatar = require('gravatar');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const keys = require('../../config/keys');

// @route   POST api/users/login
// @desc    Login User / Returning Token
// @access  Public
router.post('/login', async (req, res) => {
  const email = req.body.email;
  const password = req.body.password;

  // find user by email
  const user = await User.findOne({ email });

  // user not found
  if (!user) return res.status(404).json({ email: 'User not found' });

  // user found, check password
  const isMatch = await bcrypt.compare(password, user.password);

  // wrong password
  if (!isMatch) return res.status(400).json({ password: 'Password incorrect' });

  // correct password, create user payload
  // payload is actual data being sent over the internet
  // exclude header information
  const { id, name, avatar } = user;
  const payload = { id, name, avatar };

  // Sign Token
  jwt.sign(payload, keys.jwtSecret, { expiresIn: 3600 }, (err, token) => {
    return res.json({ success: true, token: 'Bearer ' + token });
  });
  // return res.json({ msg: 'Success' });
});

module.exports = router;
```
##### Implementing React
* npm i concurrently
* edit package.json
```javascript
{
  "name": "devconnector",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "client-install": "npm install --prefix client",
    "start": "node server.js",
    "server": "nodemon server.js",
    "client": "npm start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client\""
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "body-parser": "^1.18.3",
    "concurrently": "^3.6.1",
    "express": "^4.16.3",
    "gravatar": "^1.6.0",
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
