##### Getting Started With React
* npm i -g create-react-app
* npm i -g npm (update all npm to latest version)
* create-react-app client
* cd client
* edit client\package.json
```javascript
"proxy": "http://localhost:5000"
```
##### Assets Setup
* https://getbootstrap.com/
* https://fontawesome.com/

##### Folders
* components
    * layout
        * Navbar.js
        * Landing.js
        * Footer.js

##### Components
* Class Component: rcc
* edit client\src\components\layout\Navbar.js
```javascript
import React, { Component } from 'react';
class Navbar extends Component {
  render() {
    return (
      <nav className="navbar navbar-expand-sm navbar-dark bg-dark mb-4">
      </nav>
    );
  }
}
export default Navbar;
```
* Function Dump Component: rfc (No lifecycle, no state)
* edit client\src\components\layout\Footer.js
```javascript
import React from 'react';
export default () => {
  return (
    <footer className="bg-dark text-white mt-5 p-4 text-center">
    </footer>
  );
};
```
* edit client\src\App.js
```javascript
import React, { Component } from 'react';
import './App.css';
import Navbar from './components/layout/Navbar';
import Footer from './components/layout/Footer';
import Landing from './components/layout/Landing';

class App extends Component {
  render() {
    return (
      <div className="App">
        <Navbar />
        <Landing />
        <Footer />
      </div>
    );
  }
}
export default App;
```

##### React Router
* cd client
* npm i react-router-dom
* edit client\src\App.js
```javascript
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';

import './App.css';
import Navbar from './components/layout/Navbar';
import Footer from './components/layout/Footer';
import Landing from './components/layout/Landing';
import Register from './components/auth/Register';
import Login from './components/auth/Login';

class App extends Component {
  render() {
    return (
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
    );
  }
}

export default App;
```
##### React Router - Link
* edit client\src\components\layout\Navbar.js
```javascript
import React, { Component } from 'react';
import { Link } from 'react-router-dom';

class Navbar extends Component {
  render() {
    return (
      <nav className="navbar navbar-expand-sm navbar-dark bg-dark mb-4">
        <div className="container">
          <a className="navbar-brand" href="landing.html">
            DevConnector
          </a>
          <button
            className="navbar-toggler"
            type="button"
            data-toggle="collapse"
            data-target="#mobile-nav"
          >
            <span className="navbar-toggler-icon" />
          </button>

          <div className="collapse navbar-collapse" id="mobile-nav">
            <ul className="navbar-nav mr-auto">
              <li className="nav-item">
                <a className="nav-link" href="profiles.html">
                  {' '}
                  Developers
                </a>
              </li>
            </ul>

            <ul className="navbar-nav ml-auto">
              <li className="nav-item">
                <Link className="nav-link" to="/register">
                  Sign Up
                </Link>
              </li>
              <li className="nav-item">
                <Link className="nav-link" to="/login">
                  Login
                </Link>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    );
  }
}
export default Navbar;
```

##### Component State

