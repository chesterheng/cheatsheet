##### Querying the sequence
```javascript
const registerClicks = event => {
  if(clicks < 10){
    if(event.clientX > window.innerWidth / 2) {
      console.log(event.clientX, event.clientY);
      clicks += 1;
    }
  }
  else {
    document.removeEventListener('click', registerClicks);
  }
};

let clicks = 0;
document.addEventListener('click', registerClicks);
```
```javascript
const observer = {
  next: event => console.log(event.clientX, event.clientY),
  error: error => console.log(error),
  complete: () => console.log('Completed')
}

Rx.Observable
  .fromEvent(document, 'click')
  .filter(event => event.clientX > window.innerWidth / 2)
  .take(10)
  .subscribe(observer);
```

##### Observer Pattern
```javascript
class Observable {
  listeners = [];

  subscribe = observer => this.listeners.push(observer);
  
  unsubscribe = observer => {
    const index = this.listeners.indexOf(observer);
    this.listeners.splice(index, 1);
  }
  
  notify = value => {
    this.listeners.forEach(observer => observer.next(value));
  }
}

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription = new Observable();
subscription.subscribe(observer);
subscription.subscribe(observer);
subscription.notify('Hello');
```

##### Creating Observables
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription = Rx.Observable
  .create(observer => {
    observer.next("Simon");
    observer.next("Jen");
    observer.next("Sergi");
    observer.complete();
  })
  .subscribe(observer);
```

##### Making Ajax Calls with an Observable
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription = Rx.Observable
  .create(observer => {
    const req = new XMLHttpRequest();
    
    req.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');
    
    req.onload = () => {
      if(req.status === 200) {
        observer.next(req.response);
        observer.complete();
      }
      else {
        observer.error(new Error(req.statusText));
      }
    };
    
    req.onerror = error => observer.error(error);
    
    req.send();
  })
  .subscribe(observer);
```

##### Making Fetch Calls with an Observable
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription = Rx.Observable
  .create(observer => {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
      if(response.status === 200) {
        response.json().then(data => {
          observer.next(data);
          observer.complete();
        });      
      }
      else {
        observer.error(response.status);
      }
    })
    .catch(error => observer.error(error));
  })
  .subscribe(observer);
```

##### Creating Observables from Arrays
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription = Rx.Observable
  .from(["Simon", "Jen", "Sergi"])
  .subscribe(observer);
```
