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
* className="App"
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
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 render () {
  return (
   <div className="App">
    <Person />
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

const person = () => <p>I am Max and I am {Math.floor(Math.random() * 30)} years old!</p>;

export default person;
```

##### Working with Props
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 render () {
  return (
   <div className="App">
    // props: = { age: "28", name: "Max" }
    <Person name="Max" age="28"/>
   </div>
  ); 
 } 
}

export default App;
```
```javascript
import React from 'react';

const person = props => <p>I am {props.name} and I am {props.age} years old!</p>;

export default person;
```

##### Understanding the Children Property
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 render () {
  return (
   <div className="App">
    // props: = { age: "28", children: "My Hobbies: Racing", name: "Max" }
    <Person name="Max" age="28">My Hobbies: Racing</Person>
   </div>
  ); 
 } 
}

export default App;
```
```javascript
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


##### Understanding & Using State
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 state = { persons: [ { name: 'Max', age: 28 }, { name: 'Manu', age: 29 }, { name: 'Step', age: 26 } ] }
 
 render () {
  return (
   <div className="App">
    <button>Switch Name</button>
    <Person name={this.state.persons[0].name} age={this.state.persons[0].age}/>
    <Person name={this.state.persons[1].name} age={this.state.persons[1].age}>My Hobbies: Racing</Person>
    <Person name={this.state.persons[2].name} age={this.state.persons[2].age}/>
   </div>
  ); 
 } 
}

export default App;
```

##### Handling Events with Methods
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 state = { persons: [ { name: 'Max', age: 28 }, { name: 'Manu', age: 29 }, { name: 'Step', age: 26 } ] }
 
 switchNameHandler = () => console.log('Was clicked!');
 
 render () {
  return (
   <div className="App">
    <button onClick={this.switchNameHandler}>Switch Name</button>
    <Person name={this.state.persons[0].name} age={this.state.persons[0].age}/>
    <Person name={this.state.persons[1].name} age={this.state.persons[1].age}>My Hobbies: Racing</Person>
    <Person name={this.state.persons[2].name} age={this.state.persons[2].age}/>
   </div>
  ); 
 } 
}

export default App;
```

##### To Which Events Can You Listen?
* https://reactjs.org/docs/events.html#supported-events

##### Manipulating the State
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component { 
 state = { persons: [ { name: 'Max', age: 28 }, { name: 'Manu', age: 29 }, { name: 'Step', age: 26 } ] };
 
 switchNameHandler = () => {
  console.log('Was clicked!');
  this.setState( { persons: [ { name: 'Jane', age: 28 }, { name: 'Manu', age: 30 }, { name: 'Step', age: 26 } ] });
 };
 
 render () {
  return (
   <div className="App">
    <button onClick={this.switchNameHandler}>Switch Name</button>
    <Person name={this.state.persons[0].name} age={this.state.persons[0].age}/>
    <Person name={this.state.persons[1].name} age={this.state.persons[1].age}>My Hobbies: Racing</Person>
    <Person name={this.state.persons[2].name} age={this.state.persons[2].age}/>
   </div>
  ); 
 } 
}

export default App;
```

##### Passing Method References Between Components
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  sstate = { persons: [ { name: 'Max', age: 28 }, { name: 'Manu', age: 29 }, { name: 'Step', age: 26 } ] };

  switchNameHandler = newName => {
    console.log('Was clicked!');
    this.setState( { persons: [ { name: 'Jane', age: 28 }, { name: newName, age: 30 }, { name: 'Step', age: 26 } ] });
  };

  render() {
    return (
      <div className="App">
        <button onClick={this.switchNameHandler.bind(this, 'John')}>Switch Name</button>
        <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
        // props = { age: 29, children: "My Hobbies: Racing", click: fn(), name: "Manu" }
        <Person name={this.state.persons[1].name} age={this.state.persons[1].age} click={() => this.switchNameHandler('John')}>
          My Hobbies: Racing
        </Person>
        <Person name={this.state.persons[2].name} age={this.state.persons[2].age} />
      </div>
    );
  }
}

export default App;
```
```javascript
import React from 'react';
const person = props => {
  return (
    <div>
      <p onClick={props.click}>I am {props.name} and I am {props.age} years old!</p>
      <p>{props.children}</p>
    </div>
  );
};
export default person;
```
##### Adding Two Way Binding
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = { persons: [ { name: 'Max', age: 28 }, { name: 'Manu', age: 29 }, { name: 'Step', age: 26 } ] };

  switchNameHandler = newName => {
    console.log('Was clicked!');
    this.setState({ persons: [ { name: newName, age: 24 }, { name: 'Manu', age: 29 }, { name: 'Mary', age: 36 } ] });
  };

  nameChangedHandler = event => {
    this.setState({ persons: [ { name: 'Max', age: 24 }, { name: event.target.value, age: 29 }, { name: 'Mary', age: 25 } ] });
  };

  render() {
    return (
      <div className="App">
        <button onClick={this.switchNameHandler.bind(this, 'Jane')}>Switch Name</button>
        <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
        // props = { age: 29, changed: fn(), children: "My Hobbies: Racing", click: click(), name: "Manu" }
        <Person
          name={this.state.persons[1].name}
          age={this.state.persons[1].age}
          click={() => this.switchNameHandler('John')}
          changed={this.nameChangedHandler}>
          My Hobbies: Racing
        </Person>
        <Person name={this.state.persons[2].name} age={this.state.persons[2].age} />
      </div>
    );
  }
}

export default App;
```
```javascript
import React from 'react';
const person = props => {
  return (
    <div>
      <p onClick={props.click}>I am {props.name} and I am {props.age} years old!</p>
      <p>{props.children}</p>
      <input type="text" onChange={props.changed} value={props.name} />
    </div>
  );
};
export default person;
```

##### Adding Styling with Stylesheets
* edit src\Person\Person.css
```css
.Person {
  width: 60%;
  margin: auto;
  border: 1px solid #eee;
  box-shadow: 0 2px 3px #ccc;
  padding: 16px;
  text-align: center;
}
```
```javascript
import React from 'react';
import './Person.css';

const person = props => {
  return (
    <div className="Person">
      <p onClick={props.click}>I am {props.name} and I am {props.age} years old!</p>
      <p>{props.children}</p>
      <input type="text" onChange={props.changed} value={props.name} />
    </div>
  );
};
export default person;
```

##### Working with Inline Styles

##### Assignment 1: Time to Practice - The Base Syntax
##### Useful Resources & Links

