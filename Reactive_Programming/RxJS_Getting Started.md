##### Creating Observables
```javascript
import { Observable, of, from, fromEvent } from 'rxjs';
import { ajax } from 'rxjs/ajax';
import { allBooks, allReaders } from './data';

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('Completed')
};

// Instantiating a New Observable with the Constructor

new Observable(observer => {
// Observable.create(observer => {
  if (document.title !== 'RxBookTracer')
    observer.error('Incorrect page title.');

  allBooks.forEach(({ title }) => observer.next(title));

  setTimeout(() => observer.complete(), 2000);

  return () => console.log('Executing teardown code.');
}).subscribe(observer);

// Creating Observables from Existing Data
of('hello', 10, true, allReaders[0].name).subscribe(observer);
from(allBooks).subscribe(observer);

const button = document.querySelector('#readersButton');
const readersDiv = document.getElementById('readers');

// Creating Observables to Handle Events
fromEvent(button, 'click').subscribe(event => {
  
  // Making AJAX Requests with RxJS
  ajax.getJSON('/api/readers').subscribe(ajaxResponse => {
    ajaxResponse.forEach(({ name }) => (readersDiv.innerHTML += name + '<br>'));
  });
});
```

##### Subscribing to Observables with Observers
```javascript
import { Observable, of, from, fromEvent, concat, interval } from 'rxjs';
import { ajax } from 'rxjs/ajax';
import { map } from 'rxjs/operators';
import { allBooks, allReaders } from './data';

// Creating and Using Observers
const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const myNumbers = [1, 3, 5];
new Observable(observer => {
  if (myNumbers.length === 0) observer.error('No Values');
  myNumbers.forEach(num => observer.next(num));
  observer.complete();
}).subscribe(observer);

from(allBooks)
  .pipe(map(book => book.title))
  .subscribe(observer);

const currentTime = new Observable(observer => {
  const timeString = new Date().toLocaleTimeString();
  observer.next(timeString);
  observer.complete();
});

// Multiple Observers Executing a Single Observable
currentTime.subscribe(observer);
setTimeout(() => currentTime.subscribe(observer), 1000);
setTimeout(() => currentTime.subscribe(observer), 2000);

const timesDiv = document.getElementById('times');
const button = document.getElementById('timerButton');

const timerSubscription = interval(1000)
  .pipe(map(value => (timesDiv.innerHTML += `${new Date().toLocaleTimeString()} (${value}) <br>`)))
  .subscribe(observer);

// Cancelling Observable Execution with a Subscription
fromEvent(button, 'click').subscribe(event => timerSubscription.unsubscribe());
```


Applying Operators

Categories of Operators

Reading a Marble Diagram

Importing and Using Common Operators



Controlling the Number of Values Produced


##### Using Operators
```javascript
import { Observable, of, from, fromEvent, concat, interval, throwError } from 'rxjs';
import { ajax } from 'rxjs/ajax';
import { map, mergeAll, filter, mergeMap, tap, catchError, take, takeUntil } from 'rxjs/operators';
import { allBooks, allReaders } from './data';

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

of(1, 2, 3, 4, 5)
  .pipe(
    map(x => x * 2),
    filter(x => x > 5)
  )
  .subscribe(observer);

// Handling Errors
ajax
  .getJSON('/api/errors/500')
  .pipe(
    mergeAll(),
    filter(book => book.publicationYear < 1950),
    tap(book => console.log(book.title)),
    //catchError(error => of({ title: 'Corduroy', author: 'Don Freeman' }))
    //catchError((error, caught) => caught)
    //catchError(error => throw error.message)
    catchError(error => throwError(error.message))
  )
  .subscribe(observer);

const timesDiv = document.getElementById('times');
const button = document.getElementById('timerButton');
interval(1000)
  .pipe(
    // Controlling the Number of Values Produced
    //take(3),
    takeUntil(fromEvent(button, 'click')),
    map(value => (timesDiv.innerHTML += `${new Date().toLocaleTimeString()} (${value}) <br>`))
  )
  .subscribe(observer);
```

##### Creating Your Own Operators

##### Using Subjects and Multicasted Observables

##### Controlling Execution with Schedulers

##### Testing Your RxJS Code

