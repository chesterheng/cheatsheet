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


##### Component State

2. React-router
* use BrowserRouter when you have a server that will handle dynamic requests
* use Route component to render content based on the location’s pathname
```javascript
import React, { Component } from 'react';
import { BrowserRouter, Route } from 'react-router-dom';
import Header from './Header';
import Landing from './Landing';
import Dashboard from './Dashboard';
import SurveyNew from './surveys/SurveyNew';

class App extends Component {
    render(){
        return (
            <BrowserRouter>
                <div className="container">
                    <Header />
                    <Route exact path="/" component={Landing} />
                    <Route exact path="/surveys" component={Dashboard} />
                    <Route path="/surveys/new" component={SurveyNew} />
                </div>
            </BrowserRouter>
        );
    }
}
export default App;
```
