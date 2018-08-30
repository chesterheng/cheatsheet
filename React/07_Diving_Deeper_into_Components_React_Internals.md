#### Diving Deeper into Components & React Internals

##### A Better Project Structure
* src/api.js - You’ll probably need to make calls to a backend API at some point. Put all that code here. If it gets too unwieldy in one file, make a src/api directory and put the area-specific API files under there – like userApi.js, productApi.js, etc.
* src/components - All your Presentational (aka Dumb) components go here. These are the simple stateless ones that just take props.
* src/containers - The Container components go here. These are the stateful ones, and the ones that make the API calls. If you’re using Redux, these are the ones that are connected to the store. Notice that CSS and tests are in the same folder as their respective components.
* src/images - Put the images in one place to start with.
* src/index.js - This is where you initialize the app and call ReactDOM.render, so it makes sense to keep this at the top level.
* src/utils - You’ll probably end up with miscellaneous utility functions – error handlers, formatters, and the like. I usually put them in a file inside utils so I can access them easily.

##### Splitting an App Into Components
* App.js
```javascript
class App extends Component {
  state = {
    persons: [
      { id: 'asfa1', name: 'Max', age: 28 },
      { id: 'vasdf1', name: 'Manu', age: 29 },
      { id: 'asdf11', name: 'Stephanie', age: 26 }
    ],
    otherState: 'some other value',
    showPersons: false
  }

  nameChangedHandler = (event, id) => { ... }
  deletePersonHandler = (personIndex) => { ... }

  render() {
     return (
      <div className={classes.App}>
        <h1>Hi, I'm a React App</h1>
         <div>
          <Persons
            persons={this.state.persons}
            clicked={this.deletePersonHandler}
            changed={this.nameChangedHandler} />
        </div>
      </div>
    );
  }
}

export default App;
```
* Persons.js
```javascript
const persons = props => props.persons.map((person, index) => {
  return <Person
    click={() => props.clicked(index)}
    name={person.name}
    age={person.age}
    changed={(event) => props.changed(event, person.id)} />;
});

export default persons;
```
* Person.js
```javascript
const person = props => {
    return (
        <div className={classes.Person}>
            <p onClick={props.click}>I'm {props.name} and I am {props.age} years old!</p>
            <p>{props.children}</p>
            <input type="text" onChange={props.changed} value={props.name} />
        </div>
    )
};

export default person;
```

##### Comparing Stateless and Stateful Components
* Stateful Containers
  * class XY extends Component
    * Access to State
    * Lifecycle Hooks
  * Access State and Props via this
    * this.state.XY and this.props.XY
* Stateless
  * const XY = props => { ... }
  * Access Props via props
    * props.XY
    
##### Understanding the Component Lifecycle

##### Converting Stateless to Stateful Components
```javascript
import React from 'react';
import Person from './Person/Person';

const persons = (props) => props.persons.map( ( person, index ) => {
        return <Person
          click={() => props.clicked( index )}
          name={person.name}
          age={person.age}
          key={person.id}
          changed={( event ) => props.changed( event, person.id )} />
      } );

export default persons;
```
```javascript
import React, { Component } from 'react'
import Person from './Person/Person';

class Persons extends Component {
  render() {
    return this.props.persons.map((person, index) => {
      return <Person
        click={() => this.props.clicked(index)}
        name={person.name}
        age={person.age}
        key={person.id}
        changed={(event) => this.props.changed(event, person.id)} />
    });
  }
}

export default Persons;
```

##### Component Creation Lifecycle in Action

##### componentWillUnmount()

##### Component Updating Lifecycle Hooks

##### Component Updating Lifecycle in Action
8:06
lifecycle-update-external-learning-card.pdf
88. Updating Lifecycle Hooks (Triggered by State Changes)
3:04
lifecycle-update-internal-learning-card.pdf
cmp-deep-dive--03-should-component-update.zip
89. Performance Gains with PureComponents
10:23
cmp-deep-dive--04-pure-components.zip
90. How React Updates the App & Component Tree
2:27
91. Understanding React's DOM Updating Strategy
4:27
92. Returning Adjacent Elements (React 16+)
9:08
93. React 16.2 Feature: Fragments
94. Understanding Higher Order Components (HOCs)
4:16
95. Windows Users Must Read - File Downloads
Aux.js
Auxiliary.js
96. A Different Approach to HOCs
5:41
cmp-deep-dive--05-hocs.zip
97. Passing Unknown Props
4:06
98. Using setState Correctly
4:21
99. Validating Props
6:07
cmp-deep-dive--06-proptypes.zip
100. Available PropTypes
101. Using References ("ref")
4:57
102. More on the Ref API (React 16.3)
8:50
cmp-deep-dive--07-react-16.3-refs.zip
103. The Context API (React 16.3)
8:17
104. Updated Lifecycle Hooks (React 16.3)
5:12
cmp-deep-dive--08-react-finished.zip
105. Wrap Up
1:32
106. Useful Resources & Links
cmp-deep-dive--01-after-cmp-split.zip
cmp-deep-dive--02-added-lifecycle.zip
cmp-deep-dive--03-should-component-update.zip
cmp-deep-dive--04-pure-components.zip
cmp-deep-dive--05-hocs.zip
cmp-deep-dive--06-proptypes.zip
cmp-deep-dive--07-react-16.3-refs.zip
cmp-deep-dive--08-react-finished.zip
