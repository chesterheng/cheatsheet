##### Visualizing Observables

```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

```javascript
const subscription = Rx.Observable
  .range(1, 3)
  .subscribe(observer);
```

```javascript
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

##### Basic Sequence Operators
```javascript
const subscription = Rx.Observable
  .range(1, 5)
  .map(i => i * 2)
  .subscribe(observer);
```

```javascript
const subscription = Rx.Observable
  .range(1, 5)
  .filter(i => i % 2 === 0)
  .subscribe(observer);
```

```javascript
const subscription = Rx.Observable
  .range(1, 5)
  .reduce((sum, cur) => sum + cur, 0)
  .subscribe(observer);
```

```javascript
const subscription = Rx.Observable
  .range(1, 5)
  .reduce((prev, curr) => ({ 
    sum: prev.sum + curr, 
    count: prev.count + 1}), { sum: 0, count: 0 })
  .map(result => result.sum / result.count)
  .subscribe(observer);
```
  
```javascript
const subscriptions = [
  Rx.Observable.of(1, 2, 3),
  Rx.Observable.of(4, 5, 6),
  Rx.Observable.of(7, 8, 9)]

const subscription = Rx.Observable
  .from(subscriptions)
  .flatMap(value => value)
  .subscribe(observer);
```

```javascript
const subscriptions = [
  Rx.Observable.of(1, 2, 3),
  Rx.Observable.of(4, 5, 6),
  Rx.Observable.of(7, 8, 9)]

const subscription = Rx.Observable
  .from(subscriptions)
  .concatAll()
  .subscribe(observer);
```

##### Cancelling Sequences
```javascript
const subscription1 = Rx.Observable
  .interval(1000)
  .subscribe(observer);

const subscription2 = Rx.Observable
  .interval(1000)
  .subscribe(observer);

setTimeout(() => {
  subscription1.unsubscribe();
  subscription2.unsubscribe();
}, 2000);
```
