1 Components
* Function Component
```javascript
import React from 'react';
const Header = () => {
    return(
        <div>Header</div>
    );
};
export default Header;
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
