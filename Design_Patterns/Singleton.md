# Singleton Pattern

```javascript
class Singleton {
  constructor() {
    this.publicProperty = 'I am also public';
    this.privateRandomNumber = Math.random();
  }
  publicMethod() {
    console.log('The public can see me!');
  }
  getRandomNumber() {
    return this.privateRandomNumber;
  }
}

class SingletonFactory {
  init() {
    this.instance = new Singleton();
  }

  getInstance() {
    if (!this.instance) {
      this.init();
    }
    return this.instance;
  }
}

const singletonFactory = new SingletonFactory();
const singleA = singletonFactory.getInstance();
const singleB = singletonFactory.getInstance();
console.log(singleA === singleB);

singleA.publicMethod();
singleB.publicMethod();

console.log(singleA.publicProperty);
console.log(singleB.publicProperty);

console.log(singleA.getRandomNumber() === singleB.getRandomNumber());
```
