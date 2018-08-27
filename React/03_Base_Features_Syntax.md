#### Understanding the Base Features & Syntax

##### The Build Workflow
* Why?
  * Optimize code
  * Use next-gen JavaScript features
  * Be more productive
* How?
  * Use dependency management tool (npm or yarn)
  * Use bundler (Webpack)
  * Use compiler (babel + Presets)
  * Use development server

##### Using Create React App
* https://github.com/facebook/create-react-app
  * npm i -g create-react-app
  * C:\Users\<user>\AppData\Roaming\npm\node_modules
  * npx create-react-app my-app
  * cd my-app
  * npm start

##### Understanding the Folder Structure
* edit public\index.html
```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="root"></div>
  </body>
</html>
```
##### Understanding Component Basics
* edit src\index.js
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```
* edit src\App.js
```javascript
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
       <h1>Welcome to React</h1>
      </div>
    );
  }
}

export default App;
```

##### Understanding JSX
return React.createElement('div', {className: App}, React.createElement('h1', null, 'Welcome to React'));
```javascript
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return React.createElement('div', {className: App}, React.createElement('h1', null, 'Welcome to React'));
  }
}

export default App;
```

##### JSX Restrictions
* <div className="App">
* wrap everything into one root element per component


##### Creating a Functional Component
* "presentational", "dumb" or "stateless" components
* edit Person/Person.js
```javascript
import React from 'react';
const person = () => <p>I am a Person</p>;
export default person;
```

##### class-based Components & JSX Cheat Sheet
* "containers", "smart" or "stateful" components
```javascript
class Cmp extends Component { 
 render () {
  return (
   <div>some JSX</div>
  ); 
 } 
}
```
##### Working with Components & Re-Using Them
* edit App.js
```javascript
import React from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 render () {
  return (
   <div className="App">
    <Person />
   </div>
  ); 
 } 
}

export default App;
```

##### Outputting Dynamic Content
```javascript
import React from 'react';
const person = props => <p>I am Max and I am {Math.floor(Math.random() * 30)} years old!</p>;
export default person;
```

##### Working with Props
```javascript
import React from 'react';
const person = props => <p>I am {props.name} and I am {props.age} years old!</p>;
export default person;
```
```javascript
import React from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 render () {
  return (
   <div className="App">
    <Person name="Max" age="28"/>
   </div>
  ); 
 } 
}

export default App;
```

##### Understanding the Children Property
```javascript

/*
props: = {
 age: "28"
 children: "My Hobbies: Racing"
 name: "Max"
}
*/

import React from 'react';
const person = props => {
 return(
   <div>
    <p>I am {props.name} and I am {props.age} years old!</p>
    <p>{props.children}</p>
   </div> 
  )
};
export default person;
```
```javascript
import React from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 render () {
  return (
   <div className="App">
    <Person name="Max" age="28">My Hobbies: Racing</Person>
   </div>
  ); 
 } 
}

export default App;
```

##### Understanding & Using State
##### Props & State
##### Handling Events with Methods

##### To Which Events Can You Listen?
##### Manipulating the State

##### Functional (Stateless) vs class (Stateful) Components
3:33
44. Passing Method References Between Components
7:05
base-syntax--02-state-events.zip
45. Adding Two Way Binding
6:51
46. Adding Styling with Stylesheets
5:28
47. Working with Inline Styles
4:15
Assignment 1: Time to Practice - The Base Syntax
48. Useful Resources & Links
base-syntax--01-props-custom-cmp.zip
base-syntax--02-state-events.zip
base-syntax--03-finished.zip
Section: 4
