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
import person from './person';
import { clean, baseData } from './utility';
```

##### Understanding Classes


##### Classes, Properties and Methods

##### The Spread & Rest Operator

##### Destructuring

##### Reference and Primitive Types Refresher

##### Refreshing Array Functions
22. Wrap Up
23. Next-Gen JavaScript - Summary
next-gen-js-summary.pdf
24. JS Array Functions
