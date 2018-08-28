#### Working with Lists and Conditionals

##### Rendering Content Conditionally
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { name: 'Max', age: 28 },
      { name: 'Manu', age: 29 },
      { name: 'Step', age: 26 }
    ],
    showPersons: false
  };
  
  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  render() {
    return (
      <div className="App">
        <button onClick={this.togglePersonsHandler}>Show persons</button>
        {this.state.showPersons ? (
          <div>
            <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
            <Person
              name={this.state.persons[1].name}
              age={this.state.persons[1].age}
              click={() => this.switchNameHandler('John')}
              changed={this.nameChangedHandler}>
              My Hobbies: Racing
            </Person>
            <Person name={this.state.persons[2].name} age={this.state.persons[2].age} />
          </div>
        ) : null}
      </div>
    );
  }
}

export default App;
```

##### Handling Dynamic Content "The JavaScript Way"
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { name: 'Max', age: 28 },
      { name: 'Manu', age: 29 },
      { name: 'Step', age: 26 }
    ],
    showPersons: false
  };

  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
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

    return (
      <div className="App">
        <button style={style} onClick={this.togglePersonsHandler}>
          Show persons
        </button>
        {persons}
      </div>
    );
  }
}

export default App;
```

##### Outputting Lists
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { name: 'Max', age: 28 },
      { name: 'Manu', age: 29 },
      { name: 'Step', age: 26 }
    ],
    showPersons: false
  };

  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map(person => {
            return <Person name={person.name} age={person.age} />;
          })}
        </div>
      );
    }

    return (
      <div className="App">
        <button style={style} onClick={this.togglePersonsHandler}>
          Show persons
        </button>
        {persons}
      </div>
    );
  }
}

export default App;

```

##### Lists & State
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { name: 'Max', age: 28 },
      { name: 'Manu', age: 29 },
      { name: 'Step', age: 26 }
    ],
    showPersons: false
  };

  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  deletePersonHandler = index => {
    const persons = this.state.persons;
    persons.splice(index, 1);
    this.setState({ persons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person, index) => {
            return (
              <Person
                click={() => this.deletePersonHandler(index)}
                name={person.name}
                age={person.age}
              />
            );
          })}
        </div>
      );
    }

    return (
      <div className="App">
        <button style={style} onClick={this.togglePersonsHandler}>
          Show persons
        </button>
        {persons}
      </div>
    );
  }
}

export default App;
```

##### Updating State Immutably
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { name: 'Max', age: 28 },
      { name: 'Manu', age: 29 },
      { name: 'Step', age: 26 }
    ],
    showPersons: false
  };

  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  deletePersonHandler = index => {
    const persons = [...this.state.persons];
    persons.splice(index, 1);
    this.setState({ persons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person, index) => {
            return (
              <Person
                click={() => this.deletePersonHandler(index)}
                name={person.name}
                age={person.age}
              />
            );
          })}
        </div>
      );
    }

    return (
      <div className="App">
        <button style={style} onClick={this.togglePersonsHandler}>
          Show persons
        </button>
        {persons}
      </div>
    );
  }
}

export default App;
```

##### Lists & Keys
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { id: 'max', name: 'Max', age: 28 },
      { id: 'manu', name: 'Manu', age: 29 },
      { id: 'step', name: 'Step', age: 26 }
    ],
    showPersons: false
  };

  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  deletePersonHandler = index => {
    const persons = [...this.state.persons];
    persons.splice(index, 1);
    this.setState({ persons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person, index) => {
            return (
              <Person
                key={perons.id}
                click={() => this.deletePersonHandler(index)}
                name={person.name}
                age={person.age}
              />
            );
          })}
        </div>
      );
    }

    return (
      <div className="App">
        <button style={style} onClick={this.togglePersonsHandler}>
          Show persons
        </button>
        {persons}
      </div>
    );
  }
}

export default App;
```
##### Flexible Lists
```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      { id: 'max', name: 'Max', age: 28 },
      { id: 'manu', name: 'Manu', age: 29 },
      { id: 'step', name: 'Step', age: 26 }
    ],
    showPersons: false
  };

  nameChangedHandler = (event, id) => {
    const index = this.state.persons.findIndex(p => p.id === id);
    const person = {
      ...this.state.persons[index]
    };
    // const person = Object.assign({}, this.state.persons[index]);
    person.name = event.target.value;

    const persons = [...this.state.persons];
    persons[index] = person;

    this.setState({ persons });
  };

  togglePersonsHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  deletePersonHandler = index => {
    const persons = [...this.state.persons];
    persons.splice(index, 1);
    this.setState({ persons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person, index) => {
            return (
              <Person
                key={person.id}
                click={() => this.deletePersonHandler(index)}
                name={person.name}
                age={person.age}
                changed={event => this.nameChangedHandler(event, person.id)}
              />
            );
          })}
        </div>
      );
    }

    return (
      <div className="App">
        <button style={style} onClick={this.togglePersonsHandler}>
          Show persons
        </button>
        {persons}
      </div>
    );
  }
}

export default App;
```
```javascript
import React from 'react';
import './Person.css';

const person = props => {
  return (
    <div className="Person">
      <p onClick={props.click}>
        I am {props.name} and I am {props.age} years old!
      </p>
      <p>{props.children}</p>
      <input type="text" onChange={props.changed} value={props.name} />
    </div>
  );
};
export default person;
```

##### Wrap Up

1:55
Assignment 2: Time to Practice - Lists & Conditionals
59. Useful Resources & Links
lists-conditionals--01-conditional-content.zip
lists-conditionals--lists-finished.zip
