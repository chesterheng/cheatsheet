#### CRUD Operations Using MongoDB
##### Installing MongoDB
* https://www.mongodb.com/download-center
* install Commmunity Server and Compass

##### Connecting to MongoDB
* npm i mongoose
* edit server.js
```javascript
const mongoose = require('mongoose');
mongoose.connect(keys.mongoURI)
  .then(() => console.log('MongoDB Connected'))
  .catch(err => console.log(err));
```

##### Schemas
* String, Number, Date, Buffer, Boolean, ObjectID and Array
* edit models\Course
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

const courseSchema = new Schema({
  name: { type: String, required: true },
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean
});
module.exports = Course = mongoose.model("Course", courseSchema);
```

##### Models
```javascript
const Course = require('./models/Course');
const course = new Course({
  name: 'Node JS Course',
  author: 'John',
  tags: ['node', 'express'],
  isPublished: true
});
```

##### Saving a Document
```javascript
async createCourse = course => {
  const result = await course.save();
};
```

##### Querying Documents
```javascript
async getCourse = () => {
  const courses = await Course.find();
};

async getCourse = (query) => {
  const courses = await Course.find(query).limit(10).sort({ name: 1 }).select({ name: 1, tags: 1 });
};

const query = { author: 'John', isPublished: true }
getCourse(query)
```

##### Comparison Query Operators
* eq, ne (equal, not equal)
* gt, gte (greater than or equal)
* lt, lte (less than or equal)
* in, nin
```javascript
async getCourse = () => {
  const courses = await Course
  .find({ price: 10 })
  .find({ price: { $gte: 10, $lte: 20 } })
  .find({ price: { $in: [10, 15, 20] } })
  .limit(10).sort({ name: 1 }).select({ name: 1, tags: 1 });
};
```

##### Logical Query Operators
* or and
```javascript
async getCourse = () => {
  const courses = await Course
  .find()
  .or([ { author: 'Mosh' }, { isPublished: true }])
  and([ { } ])
  .limit(10).sort({ name: 1 }).select({ name: 1, tags: 1 });
};
```

##### Regular Expressions
```javascript
async getCourse = () => {
  const courses = await Course
  // start with Mosh
  .find({ author: /^Mosh/})
  
  // end with Hamedani
  .find({ author: /Hamedani$/})
  
  // contain Mosh
  .find({ author: /.*Mosh.*/})
  
  .limit(10).sort({ name: 1 }).select({ name: 1, tags: 1 });
};
```

##### Counting
```javascript
async getCourse = () => {
  const courses = await Course
  .find({ author: /^Mosh/})
  .limit(10)
  .sort({ name: 1 })
  .count();
};
```

##### Pagination
```javascript
async getCourse = () => {
  // /api/courses?pageNumber=2&pageSize=10
  const pageNumber = 2;
  const pageSize = 10;
  const courses = await Course
  .find({ author: /^Mosh/})
  .skip((pageNumber - 1) * pageSize)
  .limit(pageSize)
  .sort({ name: 1 })
  .count();
};
```

###### Import data to MongoDB
```exercise-data.json
[
  {"_id":"5a68fdc3615eda645bc6bdec","tags":["express","backend"],"date":"2018-01-24T21:42:27.388Z","name":"Express.js Course","author":"Mosh","isPublished":true,"price":10,"__v":0},
  {"_id":"5a68fdd7bee8ea64649c2777","tags":["node","backend"],"date":"2018-01-24T21:42:47.912Z","name":"Node.js Course","author":"Mosh","isPublished":true,"price":20,"__v":0},
  {"_id":"5a68fde3f09ad7646ddec17e","tags":["aspnet","backend"],"date":"2018-01-24T21:42:59.605Z","name":"ASP.NET MVC Course","author":"Mosh","isPublished":true,"price":15,"__v":0},
  {"_id":"5a68fdf95db93f6477053ddd","tags":["react","frontend"],"date":"2018-01-24T21:43:21.589Z","name":"React Course","author":"Mosh","isPublished":false,"__v":0},
  {"_id":"5a68fe2142ae6a6482c4c9cb","tags":["node","backend"],"date":"2018-01-24T21:44:01.075Z","name":"Node.js Course by Jack","author":"Jack","isPublished":true,"price":12,"__v":0},
  {"_id":"5a68ff090c553064a218a547","tags":["node","backend"],"date":"2018-01-24T21:47:53.128Z","name":"Node.js Course by Mary","author":"Mary","isPublished":false,"price":12,"__v":0},
  {"_id":"5a6900fff467be65019a9001","tags":["angular","frontend"],"date":"2018-01-24T21:56:15.353Z","name":"Angular Course","author":"Mosh","isPublished":true,"price":15,"__v":0}
]
```
* mongoimport --db mongo-exercises --collection courses --drop --file exercise-data.json --jsonArray

##### Updating Documents- Query First
```javascript
async updateCourse = (id) => {
  const course = await Course.findById(id);
  if(!course) return;
  if(course.isPublished) return;
  course.isPublished = true;
  course.set({
    isPublished: true
  });
  const result = await course.save();
};
```

##### Updating a Document- Update First
```javascript
async updateCourse = (id) => {
  const result = await Course.update({ _id, id},{ 
    $set: {
      author: 'Mosh',
      isPublished: false
    } 
  });
};
```
```javascript
async updateCourse = (id) => {
  const course = await Course.findByIdAndUpdate(id, { 
    $set: {
      author: 'Mosh',
      isPublished: false
    } 
  }, { new: true });
};
```
##### Removing Documents
```javascript
async removeCourse = (id) => {
  const result = await Course.deleteOne({ _id: id });
};
```
```javascript
async removeCourse = (id) => {
  const result = await Course.deleteMany({ _id: id });
  const course = await Course.findByIdAndRemove(id);
};
```
