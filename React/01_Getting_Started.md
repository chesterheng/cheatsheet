#### Getting Started

##### What is React?
* A JavaScript Library for building User Interfaces (Components)

##### Adding the Right React Version to Codepen
* https://cdnjs.cloudflare.com/ajax/libs/react/15.6.1/react.min.js 
* https://cdnjs.cloudflare.com/ajax/libs/react/15.6.1/react-dom.min.js 

##### Writing our First React Code
```html
<div id="app"></div>

<div class="person">
  <h1>Max</h1>
  <p>Your Age: 28</p>
</div>
```
```css
.person {
  display: inline-block;
  margin: 10px;
  border: 1px solid #eee;
  box-shadow: 0 2px 2px #ccc;
  width: 200px;
  padding: 20px;
}
```
```javascript
/* 
props = {
  age: "28",
  name: "Max"
}
*/
const Person = props => {
  return (
    <div className="person">
      <h1>{props.name}</h1>
      <p>Your Age: {props.age}</p>
    </div>
  );
}

// JSX must have only one root element
const app = (
  <div>
     <Person name="Max" age="28" />
     <Person name="Manu" age="29" />
  </div>
);

ReactDOM.render(app, document.querySelector('#app'));
```

##### Why Should we Choose React?
* Manage UI State
* Focus on business logic
* Huge ecosystems

##### Understanding Single Page Applications and Multi Page Applications
* Single Page Applications
  * Only ONE HTML Page, Content is (re)rendered on CLient
  * Typically only ONE ReactDOM.render() call

* Multi Page Applications
  * Multiple HTML Pages, Content is rendered in Server
  * One ReactDOM.render() call per widget

##### Course Outline
* React Basic
* Debugging
* Styling Components
* Components Deep Dive
* HTTP Requests
* Routing
* Forms and Validation
* Redux
* Authentication
* Testing
* Deployment
