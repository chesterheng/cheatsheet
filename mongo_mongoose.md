* Boliderplate: https://glitch.com/#!/import/github/freeCodeCamp/boilerplate-mongomongoose/
* Solution: https://mongo-mongoose.glitch.me/

##### MongoDB and Mongoose 
1. Install and Set Up Mongoose
* npm install --save mongodb mongoose
* edit .env
```
MONGO_URI=mongodb://<user>:<password>@ds255309.mlab.com:55309/freecodecamp
```
* edit myApp.js
```javascript
const mongoose = require('mongoose');
mongoose.connect(process.env.MONGO_URI)
```
2. Create a Model
* edit myApp.js
```javascript
const { Schema } = mongoose;
const personSchema = new Schema({
    name: {
      type: String,
      required: true
    },
    age: Number,
    favoriteFoods: [String]
  });
mongoose.model('person', personSchema);
const Person = mongoose.model('person');
```
3. Create and Save a Record of a Model
* edit myApp.js
```javascript
const handleError = (err) => console.log("Got an error", err);

const createAndSavePerson = (done) => {

  const person = new Person({
  name: "Alex",
  age: 18,
  favoriteFoods: ["Chicken Rice", "Fish Soup"]});
  
  person.save((err, data) => {
    if (err) return handleError(err);
    return done(null, data);
  });
  
  done(null /*, data*/);

};
```
