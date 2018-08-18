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

##### JWT Web Token
```javascript
const jwt = require('jsonwebtoken');

// correct password, create user payload
// payload is actual data being sent over the internet
// exclude header information
const { id, name, avatar } = user;
const payload = { id, name, avatar };

// Sign Token
jwt.sign(payload, keys.jwtSecret, { expiresIn: 3600 }, (err, token) => {
  return res.json({ success: true, token: 'Bearer ' + token });
});
```
