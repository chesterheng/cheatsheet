##### 1. Setting Up Reducer
##### 2. Switch-Case in the Reducer
* edit store\reducer.js
```javascript
import * as actionTypes from './actions';

const initialState = {
    persons: []
};

const reducer = ( state = initialState, action ) => {
    switch ( action.type ) {
        case actionTypes.ADD_PERSON:

        case actionTypes.REMOVE_PERSON:
        
    }
    return state;
};

export default reducer;
```

##### 3. Setting Up Store
##### 4. Connecting React to Redux
* edit index.js
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';

import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import reducer from './store/reducer';

// Setting Up Store
const store = createStore(reducer);

// Connecting React to Redux
ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById('root'));
registerServiceWorker();
```

##### 5. Connecting the Store to React
##### 6. Dispatching Actions from within the Component
* edit containers\Persons.js
```javascript
import React, { Component } from 'react';
import { connect } from 'react-redux';

import Person from '../components/Person/Person';
import AddPerson from '../components/AddPerson/AddPerson';
import * as actionTypes from '../store/actions';

class Persons extends Component {
    
    render () {
        return (
            <div>
                <AddPerson personAdded={this.props.onAddedPerson} />
                {this.props.prs.map(person => (
                    <Person 
                        key={person.id}
                        name={person.name} 
                        age={person.age} 
                        clicked={() => this.props.onRemovedPerson(person.id)}/>
                ))}
            </div>
        );
    }
}

const mapStateToProps = state => {
    return {
        prs: state.persons
    };
};

// Dispatching Actions from within the Component
const mapDispatchToProps = dispatch => {
    return {
        onAddedPerson: (name, age) => dispatch({type: actionTypes.ADD_PERSON, personData: {name: name, age: age}}),
        onRemovedPerson: (id) => dispatch({type: actionTypes.REMOVE_PERSON, personId: id})
    }
};

// Connecting the Store to React component
export default connect(mapStateToProps, mapDispatchToProps)(Persons);
```
##### 7. Passing and Retrieving Data with Action
* edit store\reducer.js
```javascript
import * as actionTypes from './actions';

const initialState = {
    persons: []
};

const reducer = ( state = initialState, action ) => {
    switch ( action.type ) {
        case actionTypes.ADD_PERSON:
            const newPerson = {
                id: Math.random(), // not really unique but good enough here!
                name: action.personData.name,
                age: action.personData.age
            }
            return {
                ...state,
                persons: state.persons.concat( newPerson )
            }
        case actionTypes.REMOVE_PERSON:
            return {
                ...state,
                persons: state.persons.filter(person => person.id !== action.personId)
            }
    }
    return state;
};

export default reducer;
```
