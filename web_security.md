##### hash password
```javascript
const bcrypt = require('bcryptjs');

// hash new user's password
bcrypt.genSalt(10, (err, salt) => {
  bcrypt.hash(newUser.password, salt, async (err, hash) => {
    if (err) throw err;
    newUser.password = hash;
    // save new user
    ...
  });
});
  
// check password
const isMatch = await bcrypt.compare(password, user.password);
```

##### JWT Web Token theory
1. Browser: POST /users/login with username and password
2. Server: Creates a JWT with a secret
  * Server: Returns the JWT to the browser
3. Browser: Sends the JWT on the Authorization Header
4. Server: Checks JWT signature
  * Server: Get user information from the JWT
  * Server: Sends response to the client
##### JWT Web Token implementation
* edit routes\api\users.js
```javascript
router.post('/login', async (req, res) => {
  // Browser: POST /users/login with username and password
  const email = req.body.email;
  const password = req.body.password;
  ...
  // correct password, create user payload
  // payload is actual data being sent over the internet
  // exclude header information
  const { id, name, avatar } = user;
  const payload = { id, name, avatar };

  // Server: Creates a JWT with a secret
  jwt.sign(payload, keys.jwtSecret, { expiresIn: 3600 }, (err, token) => {
    // Server: Returns the JWT to the browser
    // token = header + payload + signature
    return res.json({ success: true, token: 'Bearer ' + token });
  });
  // return res.json({ msg: 'Success' });
});
```
* test with Postman
  * POST: http://localhost:5000/api/users/login
  * Headers: KEY: Content-Type, VALUE: x-www-form-urlencoded
  * Body: 
    * KEY: email, VALUE: xxxxxx@gmail.com
    * KEY: password, VALUE: yyyyyyy
  * Response: 
```  
{
  "success": true,
  "token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjViNzdhNWE0N2QwNzY3MjNkMGU4OTRkZiIsIm5hbWUiOiJDaGVzdGVyIEhlbmciLCJhdmF0YXIiOiJodHRwczovL3MuZ3JhdmF0YXIuY29tL2F2YXRhci84NmEzNTA2Mzc5ODIyNjViZDQwZjQ1ZmJhYWQ0MTY2Nz9zPTIwMCZyPXBnJmQ9bW0iLCJpYXQiOjE1MzQ1NzY3NjQsImV4cCI6MTUzNDU4MDM2NH0.8t64P3DYnm4l9XdioNwjVtgpI1UHjN-edK5z32bogN8"
}
```  

* test with Postman
  * GET: localhost:5000/api/users/current
  * Headers: Authorization, VALUE: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjViNzdhNWE0N2QwNzY3MjNkMGU4OTRkZiIsIm5hbWUiOiJDaGVzdGVyIEhlbmciLCJhdmF0YXIiOiJodHRwczovL3MuZ3JhdmF0YXIuY29tL2F2YXRhci84NmEzNTA2Mzc5ODIyNjViZDQwZjQ1ZmJhYWQ0MTY2Nz9zPTIwMCZyPXBnJmQ9bW0iLCJpYXQiOjE1MzQ1NzY3NjQsImV4cCI6MTUzNDU4MDM2NH0.8t64P3DYnm4l9XdioNwjVtgpI1UHjN-edK5z32bogN8
  * Response: 
```  
{
  "id": "5b77a5a47d076723d0e894df",
  "name": "Chester Heng",
  "email": "chester.heng@gmail.com"
}
```
* edit config/passport.js
```javascript
const JwtStrategy = require('passport-jwt').Strategy;
const ExtractJwt = require('passport-jwt').ExtractJwt;
const mongoose = require('mongoose');
const User = mongoose.model('users');
const keys = require('../config/keys');

const opts = {};
opts.jwtFromRequest = ExtractJwt.fromAuthHeaderAsBearerToken();
opts.secretOrKey = keys.jwtSecret;

module.exports = passport => {
  passport.use(
    new JwtStrategy(opts, async (jwt_payload, done) => {
      //console.log(jwt_payload);
      const user = await User.findById(jwt_payload.id, (err, user) => {
        if (err) console.log(err);
        if (user) {
          return done(null, user);
        }
        return done(null, false);
      });
    })
  );
};
```
* edit server.js 
```javascript
const passport = require('passport');
// Passport middleware
app.use(passport.initialize());

// Passport Config
require('./config/passport')(passport);
```
```javascript
router.get('/current', passport.authenticate('jwt', { session: false }),
  (req, res) => {
    //res.json({ msg: 'Success' });
    //res.json(req.user);
    // Server: Sends response to the client
    res.json({
      id: req.user.id,
      name: req.user.name,
      email: req.user.email
    });
  }
);
```
