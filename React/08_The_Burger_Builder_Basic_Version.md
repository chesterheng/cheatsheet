#### A Real App: The Burger Builder (Basic Version)

##### Module Introduction
##### Planning an App in React - Core Steps
##### Planning our App - Layout and Component Tree
##### Planning the State

##### Setting up the Project

##### Creating a Layout Component
* https://github.com/gajus/react-aux
* https://pawelgrzybek.com/return-multiple-elements-from-a-component-with-react-16/
```html
<div id="root">
  <div>
    <div>Toolbar, SideDrawer, Backdrop</div>
    <main>
      <p>test</p>
    </main>
  </div>
</div>
```
* edit src\App.js
```javascript
import React, { Component } from 'react';
import Layout from './components/Layout/Layout';

class App extends Component {
  render() {
    return (
      <div>
        <Layout>
          <p>test</p>
        </Layout>
      </div>
    );
  }
}

export default App;
```
* edit src\components\Layout\Layout.js
```javascript
import React from 'react';
import Aux from '../../hoc/Aux';

const layout = props => (
  <Aux>
    <div>Toolbar, SideDrawer, Backdrop</div>
    <main>{props.children}</main>
  </Aux>
);

export default layout;
```
* edit src\hoc\Aux.js
```javascript
// A self-eradicating component for rendering multiple elements.
const aux = props => props.children;
export default aux;
```

##### Starting Implementation of The Burger Builder Container
```html
<div id="root">
  <div>
    <div>Toolbar, SideDrawer, Backdrop</div>
    <main>
      <div>Burger</div>
      <div>Build Controls</div>
    </main>
  </div>
</div>
```
* edit src\App.js
```javascript
import React, { Component } from 'react';
import Layout from './components/Layout/Layout';
import BurgerBuilder from './containers/BurgerBuilder/BurgerBuilder';

class App extends Component {
  render() {
    return (
      <div>
        <Layout>
          <BurgerBuilder />
        </Layout>
      </div>
    );
  }
}

export default App;
```
* edit src\containers\BurgerBuilder\BurgerBuilder.js
```javascript
import React, { Component } from 'react'
import Aux from '../../hoc/Aux';

class BurgerBuilder extends Component {
  render() {
    return (
      <Aux>
        <div>Burger</div>
        <div>Build Controls</div>
      </Aux>
    )
  }
}

export default BurgerBuilder;
```

##### Adding a Dynamic Ingredient Component
* edit src\components\Burger\BurgerIngredient\BurgerIngredient.js
```javascript
import React from 'react'
import classes from './BurgerIngredient.css';

const burgerIngredient = props => {
  let ingredient = null;
  switch (props.type) {
    case ('bread-bottom'):
      ingredient = <div className={classes.BreadBottom}></div>;
      break;
    case ('bread-top'):
      ingredient = (
        <div className={classes.BreadTop}>
          <div className={classes.Seeds1}></div>
          <div className={classes.Seeds2}></div>
        </div>
      );
      break;
    case ('meat'):
      ingredient = <div className={classes.Meat}></div>;
      break;
    case ('cheese'):
      ingredient = <div className={classes.Cheese}></div>;
      break;
    case ('bacon'):
      ingredient = <div className={classes.Bacon}></div>;
      break;
    case ('salad'):
      ingredient = <div className={classes.Salad}></div>;
      break;
    default:
      ingredient = null;
  }
  return ingredient;
};
export default burgerIngredient;
```

##### Adding Prop Type Validation
* edit src\components\Burger\BurgerIngredient\BurgerIngredient.js
```javascript
import React, { Component } from 'react';
import PropTypes from 'prop-types';
import classes from './BurgerIngredient.css';

class BurgerIngredient extends Component {
  render() {
    let ingredient = null;
    switch (this.props.type) {
      case ('bread-bottom'):
        ingredient = <div className={classes.BreadBottom}></div>;
        break;
      case ('bread-top'):
        ingredient = (
          <div className={classes.BreadTop}>
            <div className={classes.Seeds1}></div>
            <div className={classes.Seeds2}></div>
          </div>
        );
        break;
      case ('meat'):
        ingredient = <div className={classes.Meat}></div>;
        break;
      case ('cheese'):
        ingredient = <div className={classes.Cheese}></div>;
        break;
      case ('bacon'):
        ingredient = <div className={classes.Bacon}></div>;
        break;
      case ('salad'):
        ingredient = <div className={classes.Salad}></div>;
        break;
      default:
        ingredient = null;
    }
    return ingredient;
  }
}

BurgerIngredient.propTypes = {
  type: PropTypes.string.isRequired
};

export default BurgerIngredient;
```

##### Starting the Burger Component
* edit src\components\Burger\Burger.js
```javascript
import React from 'react';

import classes from './Burger.css';
import BurgerIngredient from './BurgerIngredient/BurgerIngredient';

const burger = ( props ) => {
    let transformedIngredients = Object.keys( props.ingredients )
        .map( igKey => {
            return [...Array( props.ingredients[igKey] )].map( ( _, i ) => {
                return <BurgerIngredient key={igKey + i} type={igKey} />;
            } );
        } )
        .reduce((arr, el) => {
            return arr.concat(el)
        }, []);
    if (transformedIngredients.length === 0) {
        transformedIngredients = <p>Please start adding ingredients!</p>;
    }
    return (
        <div className={classes.Burger}>
            <BurgerIngredient type="bread-top" />
            {transformedIngredients}
            <BurgerIngredient type="bread-bottom" />
        </div>
    );
};

export default burger;
```

##### Outputting Burger Ingredients Dynamically

##### Calculating the Ingredient Sum Dynamically


##### Adding the Build Control Component

##### Outputting Multiple Build Controls

##### Connecting State to Build Controls

##### Removing Ingredients Safely

##### Displaying and Updating the Burger Price

124. Adding the Order Button
10:39
18-button-code.css
burger-basics--03-after-build-controls.zip
125. Creating the Order Summary Modal
13:58
Modal.css
126. Showing & Hiding the Modal (with Animation!)
6:59
127. Implementing the Backdrop Component
8:18
128. Adding a Custom Button Component
4:46
Button.css
129. Implementing the Button Component
4:54
130. Adding the Price to the Order Summary
2:05
burger-basics--04-after-modal.zip
131. Adding a Toolbar
9:11
132. Using a Logo in our Application
6:40
burger-logo.png
133. Adding Reusable Navigation Items
11:26
134. Creating a Responsive Sidedrawer
7:44
135. Working on Responsive Adjustments
4:34
136. More about Responsive Adjustments
7:18
137. Reusing the Backdrop
9:11
138. Adding a Sidedrawer Toggle Button
6:27
139. Adding a Hamburger Icon
2:20
DrawerToggle.css
burger-basics--05-after-navigation.zip
140. Improving the App - Introduction
1:11
141. Prop Type Validation
1:32
142. Improving Performance
8:10
143. Using Component Lifecycle Methods
1:48
144. Changing the Folder Structure
4:57
145. Wrap Up
1:49
146. Useful Resources & Links
burger-basics--06-finished.zip
burger-basics--01-project-setup.zip
burger-basics--02-after-ingredients.zip
burger-basics--03-after-build-controls.zip
burger-basics--04-after-modal.zip
burger-basics--05-after-navigation.zip
Section: 9
