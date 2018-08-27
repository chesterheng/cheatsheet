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
Object {
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

const app = (
  <div>
     <Person name="Max" age="28" />
     <Person name="Manu" age="29" />
  </div>
);

ReactDOM.render(app, document.querySelector('#app'));
```

##### Why Should we Choose React?
2:03
7. React Alternatives
1:11
8. Understanding Single Page Applications and Multi Page Applications
3:38
9. Course Outline
7:28
10. How to get the Most out of This Course
2:29
11. Useful Resources & Links
Section: 2
