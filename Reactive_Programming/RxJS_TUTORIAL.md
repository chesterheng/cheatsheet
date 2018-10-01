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
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const observable = Rx.Observable.interval(1000);
observable
  .map(value => 'Number: ' + value)
  .throttleTime(1900)
  .subscribe(observer);
```

##### RxJS SUBJECT (~EventEmitter)
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const subject = new Rx.Subject();
subject.subscribe(observer);
subject.subscribe(observer);
subject.next('A new data');
subject.complete();
```

##### THE filter() OPERATOR
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const observable = Rx.Observable.interval(1000);
observable
  .filter(value => value % 2 == 0)
  .subscribe(observer);
```

##### debounceTime & distinctUntilChanged
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
  <input type="text">
  <script src="https://unpkg.com/@reactivex/rxjs@5.3.0/dist/global/Rx.js"></script>
</body>
</html>
```

```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const input = document.querySelector('input');
const observable = Rx.Observable.fromEvent(input, 'input');
observable
  .map( event => event.target.value)
  .debounceTime(500)
  .distinctUntilChanged()
  .subscribe(observer);
```

##### scan() vs reduce()
```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const observable = Rx.Observable.of(1, 2, 3, 4, 5);
observable
  .scan((prev, curr) => prev + curr, 0)
  .subscribe(observer);
```

##### pluck()
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
  <input type="text">
  <script src="https://unpkg.com/@reactivex/rxjs@5.3.0/dist/global/Rx.js"></script>
</body>
</html>
```

```javascript
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const input = document.querySelector('input');
const observable = Rx.Observable.fromEvent(input, 'input');
observable
  .pluck('target', 'value')
  .debounceTime(500)
  .distinctUntilChanged()
  .subscribe(observer);
```

##### mergeMap()
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
  <input type="text" id="first_name">
  <input type="text" id="last_name">
  <p>Full Name: <span></span></p>
<script src="https://unpkg.com/@reactivex/rxjs@5.3.0/dist/global/Rx.js"></script>
</body>
</html>
```

```javascript
const observer = {
  next: value => span.textContent = value,
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const first_name = document.querySelector('#first_name');
const last_name = document.querySelector('#last_name');

const span = document.querySelector('span');

const observable1 = Rx.Observable.fromEvent(first_name, 'input');
const observable2 = Rx.Observable.fromEvent(last_name, 'input');

observable1.mergeMap(
  event1 => observable2.map(event2 => event1.target.value + ' ' + event2.target.value)
).subscribe(observer);
```

##### switchMap()
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
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const button = document.querySelector('button');

const observable1 = Rx.Observable.fromEvent(button, 'click');
const observable2 = Rx.Observable.interval(1000);

observable1
  .switchMap(event => observable2)
  .subscribe(observer);
```

##### BehaviorSubject
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
  <div></div>
  <script src="https://unpkg.com/@reactivex/rxjs@5.3.0/dist/global/Rx.js"></script>
</body>
</html>
```

```javascript
const observer = {
  next: value => div.textContent = value,
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const button = document.querySelector('button');
const div = document.querySelector('div');

const subject = new Rx.BehaviorSubject('Not Clicked!');
button.addEventListener('click', () => subject.next('Clicked!'));

subject.subscribe(observer);
```
