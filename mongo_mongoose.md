##### MongoDB Setup With mLab
* browser: https://mlab.com/
* Create new
* Plan Type: Sandbox
* Provider: Amazon Web Services
* Region: US West
* Database Name: <projectname>db-dev or <projectname>db-prod 
* Select <projectname>db-dev or <projectname>db-prod 
* Users > Add new database user: <username>/<password>
* connect string: mongodb://<dbuser>:<dbpassword>@dsXXXXXX.mlab.com:XXXXX/<dbname>

##### Connecting To MongoDB With Mongoose
* edit dev.js
```javascript
module.exports = {
  mongoURI: 'mongodb://<username>:<password>@ds1XXXXX.mlab.com:XXXXX/<projectname>db-dev'
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
const mongoose = require('mongoose');
const keys = require('./config/keys');

// Connect to MongoDB
mongoose
  .connect(keys.mongoURI)
  .then(() => console.log('MongoDB Connected'))
  .catch(err => console.log(err));
```    

##### Creating The User Model
* edit models\User.js
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

// Create Schema
const UserSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true },
  password: { type: String, required: true },
  avatar: { type: String },
  date: { type: Date, default: Date.now }
});

module.exports = User = mongoose.model("users", UserSchema);
```
##### User Registration
* edit routes/api/users.js
```javascript
// find user by email
const user = await User.findOne({ email: req.body.email });

// user found
if (user) return res.status(400).json({ email: 'Email aready exists' });

// user not found
if (!user) return res.status(404).json({ email: 'User not found' });

// create new user
const newUser = new User({
  name: req.body.name,
  email: req.body.email,
  avatar,
  password: req.body.password
});

// save new user
await newUser.save((err, user) => {
  if (err) console.log(err);
  res.json(user);
});
```

