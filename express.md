#### Building RESTful API's Using Express
* https://medium.com/studioarmix/learn-restful-api-design-ideals-c5ec915a430f
* https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9

##### Best practices for Express app structure
* package.json – remembers all packages that your app depends on and their versions
* server.js – initializes the app and glues everything together
* databases/ – store initial seed data 
* routes/ – defines your app routes and their logic
* validation/ –  validator for JavaScript objects 
* models/ – represents data, implements business logic and handles storage
* helpers/ – code and functionality to be shared by different parts of the project
* middlewares/ – Express middlewares which process the incoming requests before handling them down to the routes
* public/ – contains all static files like images, styles and javascript
* views/ – provides templates which are rendered and served by your routes
* tests/ – tests everything which is in the other folders

##### HTTP methods
* GET: /api/courses or /api/courses/1
* POST: /api/courses 
* PUT: /api/courses/1
* DELETE: /api/courses/1

##### HTTP error code
* 400: Bad Request
* 404: Not Found

##### Building Your First Web Server
##### Environment Variables
* npm init
* npm i express
* edit server.js
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  return res.send('Hello World');
});

app.get('/api/courses', (req, res) => {
  return res.send([1, 2, 3]);
});

const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Listening on port ${port}...`));
```

##### Nodemon
* npm i nodemon
* edit package.json
```javascript
{
  "name": "before",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.16.3",
    "nodemon": "^1.18.3"
  }
}
```
* npm run dev

##### Route Parameters
```javascript
app.get('/api/courses/:id', (req, res) => {
  return res.send(req.params.id);
});
```

##### Handling HTTP GET Requests
```javascript

const courses = [
  { id: 1, name: 'course 1' },
  { id: 2, name: 'course 2' },
  { id: 3, name: 'course 3' }
];

app.get('/api/courses', (req, res) => {
  return res.send(courses);
});

app.get('/api/courses/:id', (req, res) => {
  // course exist?
  const course = courses.find(c => c.id === parseInt(req.params.id));
  
  if (!course) return res.status(404).send('Course ID not found');
  return res.send(course);
});
```

##### Handling HTTP POST Requests
```javascript
// parse body of requests with a JSON payload
app.use(express.json());
app.post('/api/courses', (req, res) => {
  const course = {
    id: courses.length + 1,
    name: req.body.name
  };
  courses.push(course);
  return res.send(course);
});
```

##### Calling Endpoints Using Postman
* Heaeder: raw, JSON(application/json
* Body: { "name": "new course" }

##### Input Validation
```javascript
const Joi = require('joi');

validateCourse = course => {
  const schema = { name: Joi.string().min(3).required() };
  /*
  { error: null,
  value: { name: 'very new' },
  then: [Function: then],
  catch: [Function: catch] }
  */
  return Joi.validate(course, schema);
};

app.post('/api/courses', (req, res) => {
  // course info valid?
  const { error } = validateCourse(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  const course = {
    id: courses.length + 1,
    name: req.body.name
  };
  courses.push(course);
  res.send(course);
});
```

##### Handling HTTP PUT Requests = Handling HTTP GET Requests + Input Validation
```javascript
app.put('/api/courses/:id', (req, res) => {
  // course exist?
  const course = courses.find(c => c.id === parseInt(req.params.id));
  if (!course) return res.status(404).send('Course ID not found');

  // course info valid?
  const { error } = validateCourse(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  // update course
  course.name = req.body.name;
  return res.send(course);
});
```

##### Handling HTTP Delete Requests
```javascript
app.delete('/api/courses/:id', (req, res) => {
  // course exist?
  const course = courses.find(c => c.id === parseInt(req.params.id));
  if (!course) return res.status(404).send('Course ID not found');

  // delete
  const index = courses.indexOf(course);
  courses.splice(index, 1);
  res.send(course);
});
```
