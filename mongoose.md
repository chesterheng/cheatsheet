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

##### Referencing Documents
##### Population
##### Embedding Documents
##### Using an Array of Sub-documents

##### Project- Build the Movies API

##### Project- Build the Movies API.zip
##### Project- Build the Rentals API

##### Project- Build the Rentals API.zip
##### Transactions

##### ObjectID

##### Validating Object ID's

##### Validating ObjectIDs.zip
##### A Better Implementation
