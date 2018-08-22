#### Node Module System

##### Global Object
```javascript
console.log(global);
```

##### Modules
```javascript
/* 
Module {
  id: '.',
  exports: {},
  parent: null,
  filename: 'C:\\Users\\Chester Heng\\Desktop\\node\\app.js',
  loaded: false,
  children: [],
  paths:
   [ 'C:\\Users\\Chester Heng\\Desktop\\node\\node_modules',
     'C:\\Users\\Chester Heng\\Desktop\\node_modules',
     'C:\\Users\\Chester Heng\\node_modules',
     'C:\\Users\\node_modules',
     'C:\\node_modules' ] }
*/
console.log(module);

// C:\Users\Chester Heng\Desktop\node
console.log(__dirname);

// C:\Users\Chester Heng\Desktop\node\app.js
console.log(__filename);
```

##### Creating a Module
* edit logger.js
```javascript
log = (message) => console.log(message);
module.exports = log;
```

##### Loading a Module
* edit app.js
```javascript
const logger = require('./logger');
logger.log();
```

##### Module Wrapper Function
```javascript
(function (exports, require, module, __filename, __dirname) {

})
```

##### Path Module
```javascript
const path = require('path');
const pathObj = path.parse(__filename);

/*
{ root: 'C:\\',
  dir: 'C:\\Users\\Chester Heng\\Desktop\\node',
  base: 'app.js',
  ext: '.js',
  name: 'app' }
*/
console.log(pathObj);
```

##### OS Module
```javascript
const os = require('os');
const totalMemory = os.totalmem();
const freeMemory = os.freemem();
console.log(`Total Memory: ${totalMemory}`);
console.log(`Total Memory: ${freeMemory}`);
```

##### File System Module
```javascript
const fs = require('fs');
const files = fs.readdirSync('./');

/*
[ 'app.js',
  'logger.js' ]
*/
console.log(files);

fs.readdir('./', (err, files) => {
  if (err) console.log('Error: ', err);
  else console.log('Result: ', files);
});
```

##### Events Module
##### Event Arguments
##### Extending Event Emitter
* edit logger.js
```javascript
const EventEmitter = require('events');
class Logger extends EventEmitter {
  log(message) {
    console.log(message);

    // rasie an event
    this.emit('messageLogged', { id: 1, url: 'http://' });
  }
}
module.exports = Logger;
```

* edit app.js
```javascript
//const EventEmitter = require('events');
//const emitter = new EventEmitter();

// register a listener
// emitter.on('messageLogged', arg => console.log('Listener called', arg));

// raise an event
// emitter.emit('messageLogged', { id: 1, url: 'http://' });

const Logger = require('./logger');
const logger = new Logger();
// register a listener
logger.on('messageLogged', arg => console.log('Listener called', arg));
logger.log('message');
```


##### HTTP Module
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  if (req.url === '/') {
    res.write('Hello World');
    res.end();
  }

  if (req.url === '/api/courses') {
    res.write(JSON.stringify([1, 2, 3]));
    res.end();
  }
});
server.on('connection', socker => console.log('New connection'));
server.listen(3000);
console.log('Listen on port 3000');
```
