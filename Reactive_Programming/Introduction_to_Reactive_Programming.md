##### Understand Reactive Programming using RxJS
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed'),
}

const source = ['1', '1', 'foo', '2', '3', '5', 'bar', '8', '13'];

Rx.Observable
  .interval(1000)
  .take(9)
  .map(i => source[i])
  .map(x => parseInt(x))
  .filter(x => !isNaN(x))
  .reduce((sum, cur) => sum + cur, 0)
  .subscribe(observer);
```
