#### Refreshing Next Generation JavaScript
* https://jsbin.com

##### Understanding "let" and "const"
* let: variable values
* const: constant values

##### Arrow Functions
```javascript
function myFnc() {
  ...
}

// no issues with this keyword
const myFnc = () => ... ;
```

##### Exports and Imports
* person.js
```javascript
const person = {
  name: 'Max'
}
export default person;
```
* utility.js
```javascript
export const clean = () => { ... };
export const baseData = 10;
```
* app.js
```javascript
// default export
import person from './person';
import prs from './person';

// named export
import { clean, baseData } from './utility';
import { clean as Clean, baseData } from './utility';
import * as bundled from './utility';
```

##### Understanding Classes
```javascript
class Human {
  constructor () {
    this.gender = 'male';
  }
  printGender() {
    console.log(this.gender);
  }
}

class Person extends Human {
  constructor () {
    super();
    this.name = 'Max';
    this.gender = 'female';
  }
  printMyName () {
    console.log(this.name);
  }
}

const person = new Person();
person.printMyName();
person.printGender();
```

##### Classes, Properties and Methods
* Properties are like variables attached to classes/objects
```javascript
// ES6
constructor () {
  this.myProperty = 'value';
}

// ES7
myProperty = 'value';
```
* Methods are like functions attached to classes/objects
```javascript
// ES6
myMethod () {...}

// ES7
myMethod = () => {...}
```
```javascript
class Human {
  gender = 'male';
  printGender = () => console.log(this.gender);
}

class Person extends Human {
  name = 'Max';
  gender = 'female';
  printMyName = () => console.log(this.name);
}

const person = new Person();
person.printMyName();
person.printGender();
```

##### The Spread & Rest Operator
* Spread: Used to split up array elements OR object properties
```javascript
const newArray = [...oldArray, 1, 2];
const newObject = { ...oldObject, newProp: 5 }

const numbers = [1, 2, 3];
const newNumbers = [...numbers, 3];

const person = { name: 'Max' };
const newPerson = { ...person, age: 28 };
```
* Rest: Used to merge a list of function arguments into an array
```javascript
function sortArgs(...args) {
  return args.sort();
}
const filter = (...args) => {
  return args.filter(el => el === 1);
}
filter(1, 2, 3);
// [1]
```

##### Destructuring
```javascript
const array = [1, 2, 3];
const [a, b] = array;
console.log(a); // prints 1
console.log(b); // prints 2
console.log(array); // prints [1, 2, 3]

const myObj = {
  name: 'Max',
  age: 28
}
const {name} = myObj;
console.log(name); // prints 'Max'
console.log(age); // prints undefined
console.log(myObj); // prints {name: 'Max', age: 28}

const printName = (personObj) => {
  console.log(personObj.name);
}
printName({name: 'Max', age: 28}); // prints 'Max'

const printName = ({name}) => {
  console.log(name);
}
printName({name: 'Max', age: 28}); // prints 'Max')
```

##### Reference and Primitive Types Refresher

##### Refreshing Array Functions

