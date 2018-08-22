##### Building RESTful API's Using Express

#### Building Your First Web Server
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

app.listen(port, () => console.log(`Listening on port ${port}...`));
```

#### Nodemon

#### Environment Variables
#### Route Parameters
#### Handling HTTP GET Requests
#### Handling HTTP POST Requests
#### Calling Endpoints Using Postman
#### Input Validation
#### Handling HTTP PUT Requests
#### Handling HTTP Delete Requests
#### Project- Build the Genres API
