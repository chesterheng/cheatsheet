* Boliderplate: https://glitch.com/#!/import/github/freeCodeCamp/boilerplate-mongomongoose/
* Solution: https://mongo-mongoose.glitch.me/

##### MongoDB and Mongoose 
1. Install and Set Up Mongoose
* npm install --save mongodb mongoose
* edit .env
```
MONGO_URI=mongodb://<user>:<password>@ds255309.mlab.com:55309/freecodecamp
```
* edit index.js
```javascript
const mongoose = require('mongoose');
mongoose.connect(process.env.MONGO_URI)
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
    await new Person({
        name: "Alex",
        age: 18,
        favoriteFoods: ["Chicken Rice", "Fish Soup"]}).save();
  });
};
```
