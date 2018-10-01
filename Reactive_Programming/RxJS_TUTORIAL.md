##### RxJS - What and Why?

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
  <button>Click Me</button>
  <script src="https://unpkg.com/@reactivex/rxjs@5.3.0/dist/global/Rx.js"></script>
</body>
</html>
```

```javascript
const button = document.querySelector('button');
//button.addEventListener('click', event => console.log(event.clientY));

Rx.Observable
  .fromEvent(button, 'click')
  .throttleTime(1000)
  .map(event => event.clientY)
  .subscribe(
    clientY => console.log(clientY)
  );
```

##### OBSERVABLES, OBSERVERS & SUBSCRIPTIONS
```javascript
const button = document.querySelector('button');
//button.addEventListener('click', event => console.log(event.target.textContent));

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subscription1 = Rx.Observable
  .create(obs => {
    obs.error('Error');
    setTimeout(() => {
      obs.complete();
    }, 2000)
    obs.next('A second value');
  })
  .subscribe(observer);
  
const subscription2 = Rx.Observable
  .create(obs => {
    button.onclick = function(event){
      obs.next(event.clientX);
    }; 
  })
  .subscribe(observer);

setTimeout(() => subscription1.unsubscribe(), 5000);
```

##### RxJS OPERATORS LIKE map() OR throttleTime()
```javascript
const observable = Rx.Observable.interval(1000);
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

observable
  .map(value => 'Number: ' + value)
  .throttleTime(1900)
  .subscribe(observer);
```
