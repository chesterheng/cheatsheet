Visualizing Observables

```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription = Rx.Observable
  .range(1, 3)
  .subscribe(observer);
```

```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscriptionA = Rx.Observable
  .interval(200)
  .map(i => 'A' + i);

const subscriptionB = Rx.Observable
  .interval(100)
  .map(i => 'B' + i);

Rx.Observable
  .merge(subscriptionA, subscriptionB)
  .subscribe(observer);
```
