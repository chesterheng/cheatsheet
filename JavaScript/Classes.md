##### ES6
```javascript
class Human {
  constructor(gender) {
    this.gender = gender;
  }
  
  printGender() {
    console.log(this.gender);
  }
}

class Person extends Human {
  constructor(name, gender) {
    super(gender);
    this.name = name;
  }
  
  printMyName() {
    console.log(this.name);
  }
}

const person = new Person('Max', 'Male');
person.printMyName();
person.printGender();
```

##### ES7
```javascript
class Human {
  gender = arguments[1];
  
  printGender = () => {
    console.log(this.gender);
  }
}

class Person extends Human {
  name = arguments[0];
 
  printMyName = () => {
    console.log(this.name);
  }
}

const person = new Person('Max', 'Male');
person.printMyName();
person.printGender();
```
