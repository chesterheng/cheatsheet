#### Building RESTful API's Using Express

##### Building Your First Web Server
##### Environment Variables
* npm init
* npm i express
* edit server.js
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.get('/api/courses', (req, res) => {
  res.send([1, 2, 3]);
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
  res.send(req.params.id);
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
  res.send(courses);
});

app.get('/api/courses/:id', (req, res) => {
  const course = courses.find(c => c.id === parseInt(req.params.id));
  // not found
  if (!course) res.status(404).send('Course ID not found');
  res.send(course);
});
```

##### Handling HTTP POST Requests
```javascript
app.use(express.json());
app.post('/api/courses', (req, res) => {
  const course = {
    id: courses.length + 1,
    name: req.body.name
  };
  courses.push(course);
  res.send(course);
});
```

##### Calling Endpoints Using Postman
* Heaeder: raw, JSON(application/json
* Body: { "name": "new course" }

##### Input Validation
```javascript
const Joi = require('joi');

app.post('/api/courses', (req, res) => {
  const schema = {
    name: Joi.string().min(3).required()
  };

  // bad request
  const result = Joi.validate(req.body, schema);
  if (result.error) return res.status(404).send(result.error.details[0].message);

  const course = { id: courses.length + 1, name: req.body.name };
  courses.push(course);
  res.send(course);
});
```

##### Handling HTTP PUT Requests
##### Handling HTTP Delete Requests
##### Project- Build the Genres API
