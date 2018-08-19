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
* <Provider> make the store available to all container components in the application without passing it explicitly. 
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
