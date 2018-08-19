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
* Function Component: rfc
```javascript
import React from 'react'
export default () => {
  return (
    <div>
      
    </div>
  )
}
```
* Class Component
```javascript
import React, { Component } from 'react';
class Header extends Component{
    render() {
        return(
            <div>Header</div>
        );
    }
}
export default Header;
```
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
