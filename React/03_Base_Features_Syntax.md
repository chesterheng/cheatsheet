#### Understanding the Base Features & Syntax

##### The Build Workflow
* Why?
  * Optimize code
  * Use next-gen JavaScript features
  * Be more productive
* How?
  * Use dependency management tool (npm or yarn)
  * Use bundler (Webpack)
  * Use compiler (babel + Presets)
  * Use development server

##### Using Create React App
* https://github.com/facebook/create-react-app
  * npm i -g create-react-app
  * C:\Users\<user>\AppData\Roaming\npm\node_modules
  * npx create-react-app my-app
  * cd my-app
  * npm start

##### Understanding the Folder Structure
Understanding Component Basics
components-learning-card.pdf
Understanding JSX
JSX Restrictions

##### Creating a Functional Component
* "presentational", "dumb" or "stateless" components
```javascript
const cmp = () => { return <div>some JSX</div> }
```

##### class-based Components & JSX Cheat Sheet
* "containers", "smart" or "stateful" components
```javascript
class Cmp extends Component { 
 render () {
  return (
   <div>some JSX</div>
  ); 
 } 
}
```
34. Working with Components & Re-Using Them
35. Outputting Dynamic Content
36. Working with Props
props-learning-card.pdf
37. Understanding the Children Property
base-syntax--01-props-custom-cmp.zip
38. Understanding & Using State
39. Props & State
props&state.pdf
40. Handling Events with Methods
3:45
41. To Which Events Can You Listen?
42. Manipulating the State
4:56
state-learning-card.pdf
43. Functional (Stateless) vs class (Stateful) Components
3:33
44. Passing Method References Between Components
7:05
base-syntax--02-state-events.zip
45. Adding Two Way Binding
6:51
46. Adding Styling with Stylesheets
5:28
47. Working with Inline Styles
4:15
Assignment 1: Time to Practice - The Base Syntax
48. Useful Resources & Links
base-syntax--01-props-custom-cmp.zip
base-syntax--02-state-events.zip
base-syntax--03-finished.zip
Section: 4
