##### MongoDB and Mongoose 
1. Install and Set Up Mongoose
* https://glitch.com/#!/import/github/freeCodeCamp/boilerplate-mongomongoose/
* npm install --save mongodb mongoose
* edit package.js
```
{
  "dependencies": {
    "mongodb": "^3.1.3",
    "mongoose": "^5.2.8"
  },
}
```
* edit .env
MONGO_URI=mongodb://chester:gis12345@ds255309.mlab.com:55309/freecodecamp
```
* edit myApp.js
```javascript
const mongoose = require('mongoose');
mongoose.connect(process.env.MONGO_URI)
```
2. Create a Model
```
const { Schema } = mongoose;
const Person = new Schema({
 name: {
    type: String,
    required: true
  },
  age : Number,
  favoriteFoods : [String]
});
```
