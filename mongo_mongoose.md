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

    



* Boliderplate: https://glitch.com/#!/import/github/freeCodeCamp/boilerplate-mongomongoose/
* Solution: https://mongo-mongoose.glitch.me/

##### MongoDB and Mongoose 
1. Install and Set Up Mongoose
* npm install --save mongodb mongoose
* edit .env
```
MONGO_URI=mongodb://<user>:<password>@ds255309.mlab.com:55309/freecodecamp
```
2. Create a Model
* edit models/Person.js
```javascript
const { Schema } = mongoose;
const personSchema = new Schema({
    name: { type: String, required: true },
    age:  Number,
    favoriteFoods: [String],
    birthDate: Date,
    employed: Boolean
  });
mongoose.model('persons', personSchema);
```
3. Create and Save a Record of a Model
* edit routes/apiRoutes.js
```javascript
const mongoose = require('mongoose');
const Person = mongoose.model('persons');

module.exports = app => {
  app.post("/api/shorturl/new", async (req, res) => {
    // get name from input element in form
    const name = req.body.name;
    // person exists?
    const existingPerson = await Person.findOne({ name: name });
    if(existingPerson) {
      return res.json({ name: name });
    }
    
    await new Person({ name: "Alex", age: 18, favoriteFoods: ["Chicken Rice", "Fish Soup"]}).save();
  });
};
```
* edit index.js
```javascript
'use strict';
const express = require('express');
const bodyParser = require('body-parser');
const mongo = require('mongodb');
const mongoose = require('mongoose');
require('./models/Person');

const app = express();
mongoose.connect(process.env.MONGO_URI);

app.use(bodyParser.urlencoded({
    extended: true
}));
app.use(cors());

require('./routes/apiRoutes')(app);

const port = process.env.PORT || 3000;
app.listen(port, function () {
  console.log('Node.js listening ...');
});
```
