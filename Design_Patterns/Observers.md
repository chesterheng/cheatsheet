# Observer Pattern
```javascript

class Subject {
  constructor() {
    this.observers = [];
  }
  subscribe(observer) {
    if (observer.notify) {
      this.observers.push(observer);
    } else {
      throw new Error('Invalid observer. notify implementation missing');
    }
  }
  unsubscribe(observer) {
    const index = this.observers.indexOf(observer);
    if (index > -1) {
      this.observers.splice(index, 1);
    }
  }
  notify(observer) {
    const index = this.observers.indexOf(observer);
    if (index > -1) {
      this.observers[index].notify(index);
    }
  }
  notifyAll() {
    this.observers.forEach(observer => observer.notify());
  }
}

class Observer {
  constructor(number) {
    this.number = number;
  }
  notify() {
    console.log('Observer ' + this.number + ' is notified!');
  }
}

const subject = new Subject();

const observer1 = new Observer(1);
const observer2 = new Observer(2);
const observer3 = new Observer(3);
const observer4 = new Observer(4);

subject.subscribe(observer1);
subject.subscribe(observer2);
subject.subscribe(observer3);
subject.subscribe(observer4);

subject.notify(observer2);
subject.unsubscribe(observer2);

subject.notifyAll();
```
