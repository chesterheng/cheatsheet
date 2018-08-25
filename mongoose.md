#### Mongoose - Data Validation

##### Validation
##### Built-In Validators
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

const courseSchema = new Schema({
  name: { 
    type: String, 
    required: true,
    minLength: 5,
    maxLength: 255,
    // match: /pattern/
  },
  category: {
    type: String,
    required: true,
    enum: ['web', 'mobile', 'network']
  }
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
  price: {
    type: Number,
    required: function () { return this.isPublished },
    min: 10,
    max: 200
  }
});

const Course = mongoose.model("Course", courseSchema);

async createCourse = () => {
  const course = new Course({
    name: 'Node JS Course',
    author: 'John',
    tags: ['node', 'express'],
    isPublished: true
  });

  try {
      const result = await course.save();
    };
  } catch(ex) {
    console.log(ex.message)
  }
}
```

##### Custom Validators
```javascript
const courseSchema = new Schema({
  tags: {
    type: Array,
    validate: {
      validator: function(v) {
        return v && v.length > 0;
      },
      message: 'A course must have at least one tag.'
    }
  },
});
```

##### Async Validators
```javascript
const courseSchema = new Schema({
  tags: {
    type: Array,
    validate: {
      isAsync: true,
      validator: function(v, callback) {
        setTimeout(() => {
          // Do some async work
          const result = v && v.length > 0;
          callback(result);
        }, 1000);
      },
      message: 'A course must have at least one tag.'
    }
  },
});
```

##### Validation Errors
```javascript
async createCourse = () => {
  const course = new Course({
    name: 'Node JS Course',
    author: 'John',
    tags: ['node', 'express'],
    isPublished: true
  });

  try {
      const result = await course.save();
    };
  } catch(ex) {
    for(field in ex.errors)
      console.log(ex.errors[field].message);
  }
}
```

##### SchemaType Options
```javascript
const courseSchema = new Schema({
  name: { 
    type: String, 
    required: true,
    minlength: 5,
    maxlength: 255,
    // match: /pattern/
  },
  category: {
    type: String,
    required: true,
    enum: ['web', 'mobile', 'network'],
    lowercase: true,
    // uppercase: true,
    trim: true
  }
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
  price: {
    type: Number,
    required: function () { return this.isPublished },
    min: 10,
    max: 200,
    get: v => Math.round(v),
    set: v => Math.round(v)
  }
});
```

##### Add Persistence to Genres API
##### Restructuring the Project
* edit models\genre.js
```javascript
const Joi = require('joi');
const mongoose = require('mongoose');
const { Schema } = mongoose;

const genreSchema = new Schema({
  name: { type: String, required: true, minlength: 5, maxlength: 50 }
});

const Genre = mongoose.model("Course", genreSchema);

const validateGenre = (genre) => {
  const schema = {
    name: Joi.string().min(3).required()
  };

  return Joi.validate(genre, schema);
}

exports.Genre = Genre; 
exports.validate = validateGenre;
```
* edit routes\genres.js
```javascript
const express = require('express');
const router = express.Router();
const {Genre, validate} = require('../models/genre');

// @route   GET api/genres
// @desc    Get all genres
// @access  Public
router.get('/', async (req, res) => {
  const genres = await Genre.find().sort('name');
  return res.send(genres);
});

// @route   GET api/genres/:id
// @desc    Get all genres
// @access  Public
router.get('/:id', async (req, res) => {
  // genre exist?
  const genre = await Genre.findById(req.params.id);
  if (!genre) return res.status(404).send('Genre ID not found');
  return res.send(genre);
});

// @route   POST api/genres
// @desc    Get all genres
// @access  Public
router.post('/', async (req, res) => {
  // genre info valid?
  const { error } = validateGenre(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  // create new genre
  let genre = new Genre({ name: req.body.name });

  // save new genre
  genre = await genre.save();
  return res.send(genre);
});

// @route   PUT api/genres/:id
// @desc    Get all genres
// @access  Public
router.put('/:id', async (req, res) => {
  // genre info valid?
  const { error } = validateGenre(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  // update genre
  const genre = await Genre.findByIdAndUpdate(
    req.params.id,
    { name: req.body.name },
    { new: true }
  );

  // genre exist?
  if (!genre) return res.status(404).send('Genre ID not found');
  return res.send(genre);
});

// @route   GET api/genres/:id
// @desc    Get all genres
// @access  Public
router.delete('/:id', async (req, res) => {
  // delete genre
  const genre = await Genre.findByIdAndRemove(req.params.id);

  if (!genre) return res.status(404).send('Genre ID not found');
  res.send(genre);
});
```

#### Mongoose- Modeling Relationships Between Connected Data
##### Modelling Relationships

##### Referencing Documents (Normalization) -> CONSISTENCY
##### Population
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

const authorSchema = new mongoose.Schema({ name: String, bio: String, website: String });
const Author = mongoose.model('Author', authorSchema);

const courseSchema = new mongoose.Schema({
  name: String,
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'Author' }
});
const Course = mongoose.model('Course', courseSchema);

const createAuthor = async (name, bio, website) => { 
  const author = new Author({ name, bio, website });
  const result = await author.save();
  console.log(result);
}

const createCourse = async (name, author) => {
  const course = new Course({ name, author }); 
  const result = await course.save();
  console.log(result);
}

const listCourses = async () => { 
  const courses = await Course
    .find()
    .populate('author', 'name -_id')
    .select('name author');
  console.log(courses);
}

createAuthor('Mosh', 'My bio', 'My Website');
createCourse('Node Course', 'authorId')
listCourses();
```

##### Embedding Documents (Denormalization) -> PERFORMANCE
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

const authorSchema = new mongoose.Schema({ name: String, bio: String, website: String });
const Author = mongoose.model('Author', authorSchema);

const courseSchema = new mongoose.Schema({
  name: String,
  author: { type: authorSchema, required: true }
});
const Course = mongoose.model('Course', courseSchema);

const createCourse = async (name, author) => {
  const course = new Course({ name, author }); 
  const result = await course.save();
  console.log(result);
}

const listCourses = async () => { 
  const courses = await Course.find();
  console.log(courses);
}

const updateAuthor = courseId => {
  const course = await Course.findById(courseId);
  course.author.name = 'Mosh Hamedani';
  course.save();
}

const updateAuthor = courseId => {
  const course = await Course.update({ _id: courseId }, {
    $set: {
      'author.name': 'John Smith'
    }
    $unset: {
      'author': ''
    }
  });
}

const author = new Author('Mosh', 'My bio', 'My Website');
createCourse('Node Course', author);
listCourses();
```

##### Using an Array of Sub-documents
```javascript
const courseSchema = new mongoose.Schema({
  name: String,
  author: [authorSchema]
});
const Course = mongoose.model('Course', courseSchema);

const createCourse = async (name, authors) => {
  const course = new Course({ name, authors }); 
  const result = await course.save();
  console.log(result);
}

const addAuthor = async (courseId, author) => {
  const course = await Course.findById(courseId);
  course.authors.push(author);
  await course.save();
}

const removeAuthor = async (courseId, authorId) => {
  const course = await Course.findById(courseId);
  const author = course.authors.id(authorId);
  author.remove();
  await course.save();
}

createCourse('Node Course', [
  new Author({ name: 'Mosh' }),
  new Author({ name: 'John' })
]);
```

##### Transactions
* 2 phase commit
* npm i fawn
const Fawn = required('fawn');

Fawn.init(mongoose);

try {
  new Fawn.Task()
    .save('rentals', rental)
    .update('movies', { _id: movie._id }, {
      $inc: { numberInStock: -1 }
    })
    .run();
    
    res.send(rental);
} catch(err) {
  res.status(500).send('Something failed.');
}

##### ObjectID (12 bytes)
* 4 bytes: timestamp
* 3 bytes: machine ID
* 2 bytes: process ID
* 3 bytes: counter
* Driver -> MongoDB
```javascript
const mongoose = require('mongoose');

const id = new mongoose.Types.ObjectId();
console.log(id.getTimestamp());

const isValid = mongoose.Types.ObjectId.isValid('1234');
console.log(isValid);
```

##### Validating Object ID's
##### A Better Implementation
* edit server.js
```javascript
const Joi = require('joi');
Joi.objectId = require('joi-objectid')(Joi);
```
* edit models\rental.js
```javascript
const validateRental = (rental) => {
  const schema = {
    customerId: Joi.objectId().required(),
    movieId: Joi.objectId().required()
  };

  return Joi.validate(rental, schema);
}
```
* edit routes\movies.js
```javascript
  const movie = new Movie({ 
    title: req.body.title,
    genre: {
      _id: genre._id,
      name: genre.name
    },
    numberInStock: req.body.numberInStock,
    dailyRentalRate: req.body.dailyRentalRate
  });
  await movie.save();
  ```
  
