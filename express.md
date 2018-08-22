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

const port = 3000 || process.env.PORT;
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

##### Handling HTTP GET Requests
##### Handling HTTP POST Requests
##### Calling Endpoints Using Postman
##### Input Validation
##### Handling HTTP PUT Requests
##### Handling HTTP Delete Requests
##### Project- Build the Genres API
