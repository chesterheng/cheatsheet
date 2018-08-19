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

http://www.codingpedia.org/ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example#after-1
##### Create

##### Read
* Get current user profile
```javascript
router.get('/', async (req, res) => {
    try {
      const profile = await Profile.findOne({ user: req.user.id }).populate( 'user', ['name', 'avatar'] );
      if (!profile) {
        return res.status(404).json({ noprofile: 'There is no profile for this user' });
      }
      return res.json(profile);
    } catch (err) {
      return res.status(404).json({ noprofile: 'There is no profile for this user' });
    }
  }
);
```
* Get all profiles
```javascript
router.get('/all', async (req, res) => {
  try {
    const profiles = await Profile.find().populate('user', ['name', 'avatar']);
    if (!profiles) {
      return res.status(404).json({ noprofile: 'There are no profiles' });
    }
    return res.json(profiles);
  } catch (err) {
    return res.status(404).json({ noprofile: 'There are no profiles' });
  }
});
```
* Get profile by handle
```javascript
router.get('/handle/:handle', async (req, res) => {
  try {
    const errors = {};
    const profile = await Profile.findOne({ handle: req.params.handle }).populate('user', ['name', 'avatar']);
    if (!profile) {
      return res.status(404).json({ noprofile: 'There is no profile for this user' });
    }
    return res.json(profile);
  } catch (err) {
    res.status(500).json({ error: 'Unknown server error' });
  }
});
```
##### Update
* Create or Edit user profile
```javascript
router.post('/', async (req, res) => {

    // Get fields
    const profileFields = {};
    profileFields.user = req.user.id;
    if (req.body.handle) profileFields.handle = req.body.handle;
    if (req.body.company) profileFields.company = req.body.company;
    if (req.body.website) profileFields.website = req.body.website;
    if (req.body.location) profileFields.location = req.body.location;
    if (req.body.bio) profileFields.bio = req.body.bio;
    if (req.body.status) profileFields.status = req.body.status;
    if (req.body.githubusername)
      profileFields.githubusername = req.body.githubusername;
    // Skills - Spilt into array
    if (typeof req.body.skills !== 'undefined') {
      profileFields.skills = req.body.skills.split(',');
    }

    // Social
    profileFields.social = {};
    if (req.body.youtube) profileFields.social.youtube = req.body.youtube;
    if (req.body.twitter) profileFields.social.twitter = req.body.twitter;
    if (req.body.facebook) profileFields.social.facebook = req.body.facebook;
    if (req.body.linkedin) profileFields.social.linkedin = req.body.linkedin;
    if (req.body.instagram) profileFields.social.instagram = req.body.instagram;

    let profile = await Profile.findOne({ user: req.user.id });
    if (profile) {
      // Update
      profile = await Profile.findOneAndUpdate({ user: req.user.id }, { $set: profileFields }, { new: true });
      return res.json(profile);
    } else {
      // Create

      // Check if handle exists
      profile = await Profile.findOne({ handle: profileFields.handle });
      if (profile) {
        return res.status(400).json({ handle: 'That handle already exists' });
      }

      // Save Profile
      profile = await new Profile(profileFields).save();
      return res.json(profile);
    }
  }
);
```
* Add experience to profile
```javascript
router.post('/education', async (req, res) => {
    let profile = await Profile.findOne({ user: req.user.id });
    const newEdu = {
      school: req.body.title,
      degree: req.body.company,
      fieldofstudy: req.body.location,
      from: req.body.from,
      to: req.body.to,
      current: req.body.current,
      description: req.body.description
    };

    // add edu array
    profile.education.unshift(newEdu);
    profile = await profile.save();
    res.json(profile);
  }
);
```

##### Delete
* Delete profile
```javascript
router.delete('/', async (req, res) => {
    try {
      // delete profile
      const profile = await Profile.findOneAndRemove({ user: req.user.id });
      if (!profile) {
        return res.status(404).json({ noprofile: 'Profile Not Found' });
      } else {
        return res.status(204).json({ success: 'Profile successfully deleted' });
      }
      
      // delete user
      const user = await User.findOneAndRemove({ _id: req.user.id });
      if (!user) {
        return res.status(404).json({ noprofile: 'User Not Foundr' });
      } else {
        return res.status(204).json({ success: 'User successfully deleted' });
      }
    } catch (err) {
      res.status(500).json({ error: 'Unknown server error' });
    }
  }
);
```
* Delete experience from profile
```javascript
router.delete('/experience/:exp_id', async (req, res) => {
    try {
      const profile = await Profile.findOne({ user: req.user.id });

      // get remove index
      const removeIndex = profile.experience.map(item => item.id).indexOf(req.params.exp_id);

      // splice out of array
      profile.experience.splice(removeIndex, 1);

      // save
      profile = profile.save();
      return res.json(profile);
    } catch (err) {
      res.status(404).json(err);
    }
  }
);
