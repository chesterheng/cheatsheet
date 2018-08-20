##### Create a Redux Store
```javascript
const reducer = (state = 5) => state;
const store = Redux.createStore(reducer);
```

##### Get State from the Redux Store
```javascript
const reducer = (state = 5) => state;
const store = Redux.createStore(reducer);
const currentState = store.getState();
```

##### Define a Redux Action
```javascript
const action = { type: 'LOGIN' }
```

##### Define an Action Creator
```javascript
const action = { type: 'LOGIN' }
const actionCreator = () => action;
```

##### Dispatch an Action Event
```javascript
const reducer = (state = {login: false}) => state;
const store = Redux.createStore(reducer);
const loginAction = () => ({ type: 'LOGIN' });
store.dispatch(loginAction());
```

##### Handle an Action in the Store
```javascript
const defaultState = { login: false };
const reducer = (state = defaultState, action) => {
  if(action.type === 'LOGIN') return { login: true };
  return state;
};
const store = Redux.createStore(reducer);
const loginAction = () => ({ type: 'LOGIN' });
```

##### Use a Switch Statement to Handle Multiple Actions
```javascript
const defaultState = { authenticated: false };
const authReducer = (state = defaultState, action) => {
  switch(action.type){
    case 'LOGIN': return { authenticated: true };
    case 'LOGOUT': return { authenticated: false };
  default: return state;
  }
};
const store = Redux.createStore(authReducer);
const loginUser = () => ({ type: 'LOGIN' });
const logoutUser = () => ({ type: 'LOGOUT' });
```

##### Use const for Action Types
```javascript
const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';
const defaultState = { authenticated: false };
const authReducer = (state = defaultState, action) => {
  switch (action.type) {
    case LOGIN: return { authenticated: true }
    case LOGOUT: return { authenticated: false }
    default: return state;
  }
};
const store = Redux.createStore(authReducer);
const loginUser = () => ({ type: LOGIN });
const logoutUser = () => ({ type: LOGOUT });
```

##### Register a Store Listener
```javascript
const ADD = 'ADD';
const reducer = (state = 0, action) => {
  switch(action.type) {
    case ADD: return state + 1;
    default: return state;
  }
};
const store = Redux.createStore(reducer);
let count = 0;
store.subscribe(() => count += 1);
store.dispatch({type: ADD});
console.log(count);
store.dispatch({type: ADD});
console.log(count);
store.dispatch({type: ADD});
console.log(count);
```

##### Combine Multiple Reducers
```javascript
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';
const counterReducer = (state = 0, action) => {
switch(action.type) {
  case INCREMENT: return state + 1;
  case DECREMENT: return state - 1;
  default: return state;
  }
};

const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';
const authReducer = (state = {authenticated: false}, action) => { 
  switch(action.type) {
    case LOGIN: return { authenticated: true }
    case LOGOUT: return { authenticated: false }
    default: return state;
  }
};

const rootReducer = Redux.combineReducers({
  count: counterReducer,
  auth: authReducer 
});
const store = Redux.createStore(rootReducer);
```

##### Send Action Data to the Store
```javascript
const ADD_NOTE = 'ADD_NOTE';
const notesReducer = (state = 'Initial State', action) => {
  switch(action.type) {
    case ADD_NOTE: return action.text;
    default: return state;
  }
};

const addNoteText = (note) => ({ type: ADD_NOTE, text: note });
const store = Redux.createStore(notesReducer);
console.log(store.getState());
store.dispatch(addNoteText('Hello!'));
console.log(store.getState());
```

##### Use Middleware to Handle Asynchronous Actions
```javascript
const REQUESTING_DATA = 'REQUESTING_DATA';
const RECEIVED_DATA = 'RECEIVED_DATA';

const requestingData = () => ({ type: REQUESTING_DATA });
const receivedData = (data) => ({ type: RECEIVED_DATA, users: data.users });

// use Redux Thunk middleware to handlle asynchronous types of requests
// return dispatch function after call back
// By default, Redux action creators don't support asynchronous actions
// so we utilise Redux Thunk 
// Thunk allows you to write action creators that return a function instead of an action. 
// The inner function can receive the store methods dispatch and getState as parameters, but we'll just use dispatch.
const handleAsync = () => dispatch => {
  // dispatch request action here
  dispatch(requestingData());
  setTimeout(() => {
    let data = { users: ['Jeff', 'William', 'Alice'] };
    // dispatch received data action here
    dispatch(receivedData(data));
  }, 2500);
};
const defaultState = { fetching: false, users: [] };

const asyncDataReducer = (state = defaultState, action) => {
  switch(action.type) {
    case REQUESTING_DATA: return { fetching: true, users: [] }
    case RECEIVED_DATA: return { fetching: false, users: action.users } default: return state;
  }
};
const store = Redux.createStore( asyncDataReducer, Redux.applyMiddleware(ReduxThunk.default) );
```

##### Write a Counter with Redux
```javascript
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';
const counterReducer = (state = 0, action) => {
  switch(action.type) {
    case INCREMENT: return state+1;
    case DECREMENT: return state-1;
    default: return state;
  }
};
const incAction = () => ({ type: INCREMENT });
const decAction = () => ({ type: DECREMENT });
const store = Redux.createStore(counterReducer);
```

##### Never Mutate State
```javascript
const ADD_TO_DO = 'ADD_TO_DO';
const todos = [ 'Go to the store', 'Clean the house', 'Cook dinner', 'Learn to code'];
const immutableReducer = (state = todos, action) => {
  switch(action.type) {
    case ADD_TO_DO: return state.concat(action.todo);
    default: return state;
  }
};
const addToDo = (todo) => ({ type: ADD_TO_DO, todo });
const store = Redux.createStore(immutableReducer);
```

##### Use the Spread Operator on Arrays
```javascript
const immutableReducer = (state = ['Do not mutate state!'], action) => { 
  switch(action.type) {
    case 'ADD_TO_DO': return [...state, action.todo];
    default: return state;
  }
};
const addToDo = (todo) => ({ type: 'ADD_TO_DO', todo });
const store = Redux.createStore(immutableReducer);
```

##### Remove an Item from an Array
```javascript
const immutableReducer = (state = [0,1,2,3,4,5], action) => {
  switch(action.type) {
    case 'REMOVE_ITEM': return [...state.slice(0, action.index), ...state.slice(action.index + 1, state.length)];
    //case 'REMOVE_ITEM': return state.slice(0, action.index).concat(state.slice(action.index + 1, state.length));
    default: return state;
  }
};
const removeItem = (index) => ({ type: 'REMOVE_ITEM', index });
const store = Redux.createStore(immutableReducer);
```

##### Copy an Object with Object.assign
```javascript
const defaultState = { user: 'CamperBot', status: 'offline', friends: '732,982', community: 'freeCodeCamp' };
const immutableReducer = (state = defaultState, action) => {
  switch(action.type) {
    case 'ONLINE': return Object.assign({}, state, { status: 'online' }); 
    default: return state;
  }
};
const wakeUp = () => ({ type: 'ONLINE' });
const store = Redux.createStore(immutableReducer);
```
