#### Introduction to the Basic Node and Express Challenges
* edit myApp.js
```javascript
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const app = express();

// --> 7)  Mount the Logger middleware here
const myLogger = (req, res, next) => {
  console.log(`${req.method} ${req.path} - ${req.ip}`); 
  next();
};

app.use(myLogger);

// --> 11)  Mount the body-parser middleware  here
app.use(bodyParser.urlencoded({     // to support URL-encoded bodies
  extended: false
})); 

/** 1) Meet the node console. */
console.log("Hello World");

/** 2) A first working Express Server */
/*app.get("/", (req, res) => {
  res.send("Hello Express");
});*/

/** 3) Serve an HTML file */
app.get("/", (req, res) => {
  const absolutePath = path.join(__dirname, "/views/index.html");
  res.sendFile(absolutePath);
});

/** 4) Serve static assets  */
const publicPath = path.join(__dirname, "/public");
app.use("/", express.static(publicPath));

/** 5) serve JSON on a specific route */
/*app.get("/json", (req, res) => {
  res.json({"message": "Hello json"});
});*/

/** 6) Use the .env file to configure the app */
app.get("/json", (req, res) => {
  let data = {"message": "Hello json"};
  if(process.env.MESSAGE_STYLE === 'uppercase'){
    data.message = data.message.toUpperCase();
  }
  res.json(data);
});
 
/** 7) Root-level Middleware - A logger */
//  place it before all the routes !


/** 8) Chaining middleware. A Time server */
const requestTime = (req, res, next) => {
  req.time = new Date().toString();
  next();
};

app.get("/now", requestTime, (req, res) => {
  res.send({time: req.time});
});

/** 9)  Get input from client - Route parameters */
app.get("/:word/echo", (req, res) => res.json({echo: req.params.word}));

/** 10) Get input from client - Query parameters */
// /name?first=<firstname>&last=<lastname>
app.route('/name').get((req, res) => {
  let firstname = req.query.first;
  let lastname = req.query.last;
  let name = `${firstname} ${lastname}`;
  res.send({ name: name });
}).post();
  
/** 11) Get ready for POST Requests - the `body-parser` */
// place it before all the routes !

/** 12) Get data form POST  */
app.post('/name', (req, res) => {
  let firstname = req.body.first;
  let lastname = req.body.last;
  let name = `${firstname} ${lastname}`;
  res.json({ name: name });
});


// This would be part of the basic setup of an Express app
// but to allow FCC to run tests, the server is already active
/** app.listen(process.env.PORT || 3000 ); */

//---------- DO NOT EDIT BELOW THIS LINE --------------------

 module.exports = app;
```
