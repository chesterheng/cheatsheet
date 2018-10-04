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

##### Using Operators

##### Creating Your Own Operators

##### Using Subjects and Multicasted Observables

##### Controlling Execution with Schedulers

##### Testing Your RxJS Code

