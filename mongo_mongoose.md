Boliderplate: https://glitch.com/#!/import/github/freeCodeCamp/boilerplate-mongomongoose/
Solution: https://mongo-mongoose.glitch.me/

##### MongoDB and Mongoose 
1. Install and Set Up Mongoose
* npm install --save mongodb mongoose
* edit package.js
```
{
  "name": "fcc-mongo-mongoose-challenges",
  "version": "0.0.1",
  "description": "A boilerplate project",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.12.4",
    "body-parser": "^1.15.2",
    "mongodb": "^3.1.3",
    "mongoose": "^5.2.8"
  },
  "engines": {
    "node": "4.4.5"
  },
  "repository": {
    "type": "git",
    "url": "https://hyperdev.com/#!/project/welcome-project"
  },
  "keywords": [
    "node",
    "hyperdev",
    "express"
  ],
  "license": "MIT"
}
```
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
```
const { Schema } = mongoose;
const Person = new Schema({
    name: {
      type: String,
      required: true
    },
    age: Number,
    favoriteFoods: [String]
  });
```
