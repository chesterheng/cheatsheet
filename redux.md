##### Why We Need Redux
* Share data among components
* UI -> Action -> Reducer -> Store -> UI

##### Redux Store & Chrome Extension Setup
* npm i redux react-redux redux-think

#### Create Reducers
* edit client\src\reducers\authReducer.js
```javascript
const initialState = {
  isAuthenticated: false,
  user: {}
};

export default (state = initialState, action) => {
  switch (action.type) {
    default:
      return state;
  }
};
```
* edit client\src\reducers\index.js
```javascript
import { combineReducers } from 'redux';
import authReducer from './authReducer';

export default combineReducers({
  auth: authReducer
});
```
#### Create Store
```javascript
state = {
  auth: {
    isAuthenticated: false,
    user: {}
  }
}
```
* edit client\src\store.js
```javascript
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';

const initialState = {};
const middleware = [thunk];

const store = createStore(
  rootReducer,
  initialState,
  // Compose is used when you want to pass multiple store enhancers to the store. 
  // Store enhancers are higher order functions that add some extra functionality to the store.
  // Higher order functions can take functions as parameters and return functions as return values.
  // func1(func2(func3(func4))))
  // compose(func1, func2, func3, func4)
  compose(
    applyMiddleware(...middleware),
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
  )
);
export default store;
```
#### Passing the Store
* Provider component make the store available to all container components in the application without passing it explicitly. 
* You only need to use it once when you render the root component.
* edit client\src\index.js
```javascript
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import { Provider } from 'react-redux';

import store from './store';
import './App.css';
import Navbar from './components/layout/Navbar';
import Footer from './components/layout/Footer';
import Landing from './components/layout/Landing';
import Register from './components/auth/Register';
import Login from './components/auth/Login';

class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <Router>
          <div className="App">
            <Navbar />
            <Route exact path="/" component={Landing} />
            <div className="container">
              <Route exact path="/register" component={Register} />
              <Route exact path="/login" component={Login} />
            </div>
            <Footer />
          </div>
        </Router>
      </Provider>
    );
  }
}

export default App;
```
##### Redux Action & Reducer Workflow Example

#### Connect component to redux store
* edit client\src\components\auth\Register.js
* connect function bridge between dumb React components and the Flux data flow of Redux.
```javascript
import React, { Component } from 'react';
import axios from 'axios';
import classnames from 'classnames';
import { connect } from 'react-redux';
import { registerUser } from '../../actions/authActions';

class Register extends Component {
  state = {
    name: '',
    email: '',
    password: '',
    password2: '',
    errors: {}
  };

  onChange = e => {
    this.setState({ [e.target.name]: e.target.value });
  };

  onSubmit = async e => {
    e.preventDefault();
    const newUser = {
      name: this.state.name,
      email: this.state.email,
      password: this.state.password,
      password2: this.state.password2
    };

    this.props.registerUser(newUser);
  };

  render() {
    const { errors } = this.state;
    return (...);
  }
}

export default connect(
  null,
  { registerUser }
)(Register);
```

#### Create Action
* edit client\src\actions\types.js
```javascript
export const TEST_DISPATCH = 'TEST_DISPATCH';
```
* edit client\src\actions\authActions.js
```javascript
import { TEST_DISPATCH } from './types';

// Register User
export const registerUser = userData => {
  return {
    type: TEST_DISPATCH,
    payload: userData
  };
};
```

#### Create Reducer
* edit client\src\e\reducers\authReducer.js
```javascript
import { TEST_DISPATCH } from '../actions/types';

const initialState = {
  isAuthenticated: false,
  user: {}
};

export default (state = initialState, action) => {
  switch (action.type) {
    case TEST_DISPATCH:
      return {
        ...state,
        user: action.payload
      };
    default:
      return state;
  }
};
```
* edit client\src\e\reducers\index.js
```javascript
import { combineReducers } from 'redux';
import authReducer from './authReducer';

export default combineReducers({
  auth: authReducer
});
```

#### Press UI Submit Button
* this.props.registerUser(newUser);
* action object created
```javascript
action {
  type: 'TEST_DISPATCH',
  payload: {
    name: '',
    email: '',
    password: '',
    password2: ''
  }
}
```
* connect(): inject store.dispatch() implicitly or with mapDispatchToProps()
* connect(): getState() is also called from dispatch
* store object updated
```javascript
state = {
  auth: {
    isAuthenticated: false,
    user: {
      name: '',
      email: '',
      password: '',
      password2: ''
    }
  }
}
```
* connect(): re-render component with new state
```
import React, { Component } from 'react';
import PropTypes from 'prop-types';
import axios from 'axios';
import classnames from 'classnames';
import { connect } from 'react-redux';
import { registerUser } from '../../actions/authActions';

class Register extends Component {
  state = {
    name: '',
    email: '',
    password: '',
    password2: '',
    errors: {}
  };

  onChange = e => {
    this.setState({ [e.target.name]: e.target.value });
  };

  onSubmit = async e => {
    e.preventDefault();
    const newUser = {
      name: this.state.name,
      email: this.state.email,
      password: this.state.password,
      password2: this.state.password2
    };
    // dispatch
    this.props.registerUser(newUser);
  };

  render() {
    const { errors } = this.state;
    // results of mapStateToProps are merged into the component’s props
    const { user } = this.props.auth;
    return (...);
  }
}

// Typechecking With PropTypes
Register.propTypes = {
  registerUser: PropTypes.func.isRequired,
  auth: PropTypes.object.isRequired
};

// any time the store is updated, mapStateToProps will be called. 
const mapStateToProps = state => ({ auth: state.auth });

// Connects a React component to a Redux store
export default connect(
  mapStateToProps,
  { registerUser }
)(Register);

```
