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
```javascript
import { Observable, of, from, fromEvent, concat, interval, throwError, Subscription } from 'rxjs';
import { ajax } from 'rxjs/ajax';
import { mergeMap, filter, tap, catchError, take, takeUntil, map, mergeAll } from 'rxjs/operators';
import { allBooks, allReaders } from './data';

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

const doublerOperator = () => map(value => value * 2);
of(1, 2, 3, 4, 5)
  .pipe(doublerOperator())
  .subscribe(observer);

// Creating New Operators with the Observable Constructor
const grabAndLogClassics = (year, log) => subscription =>
  new Observable(observer => {
    subscription.subscribe(
      book => {
        if (book.publicationYear < year) {
          observer.next(book);
          if (log) {
            console.log(book.title);
          }
        }
      },
      error => observer.error(error),
      complete => observer.complete()
    );
  });

// Creating New Operators from Existing Operators
const grabClassics = year => filter(book => book.publicationYear < year);

const grabAndLogClassicsWithPipe = (year, log) => subscription =>
  subscription.pipe(
    filter(book => book.publicationYear < year),
    tap(book => (log ? console.log(book.title) : null))
  );

ajax
  .getJSON('/api/books')
  .pipe(
    mergeAll(),
    //grabClassics(1950),
    //grabAndLogClassics(1930, false),
    grabAndLogClassicsWithPipe(1950, false),
    map(book => book.title)
  )
  .subscribe(observer);
```

##### Using Subjects and Multicasted Observables
```javascript
import { Observable, of, from, fromEvent, concat, interval, throwError, Subject } from 'rxjs';
import { ajax } from 'rxjs/ajax';
import { mergeMap, filter, tap, catchError, take, takeUntil, multicast, refCount, publish, share, publishLast, publishBehavior, publishReplay
} from 'rxjs/operators';
import { allBooks, allReaders } from './data';

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

// Observable -> Subject -> Observer1 and Observer2
const subject = new Subject -> ();
subject.subscribe(observer);
subject.subscribe(observer);
subject.next('Hello');

const subscription = new Observable(observer => observer.next('Greetings'));
subscription.subscribe(subject);
//subscription.subscribe(observer);
//subscription.subscribe(observer);

// Using a Subject to Convert an Hot Observable
// Observable -> Subject -> Observer1 and Observer2
const source = interval(1000).pipe(take(4));
source.subscribe(subject);

subject.subscribe(observer);
setTimeout(() => subject.subscribe(observer), 1000);
setTimeout(() => subject.subscribe(observer), 2000);

// Multicasting Operators - multicast and connect
const source = interval(1000).pipe(
  take(4),
  multicast(new Subject()),
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);
source.connect();

// Multicasting Operators - multicast and refCount
const source = interval(1000).pipe(
  take(4),
  multicast(new Subject()),
  refCount()
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);

// Multicasting Operators - publish and refCount
const source = interval(1000).pipe(
  take(4),
  publish(),
  refCount()
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);

// Multicasting Operators - share (re-subscribe late observer)
const source = interval(1000).pipe(
  take(4),
  share()
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);

// AsyncSubject
const source = interval(1000).pipe(
  take(4),
  publishLast(),
  refCount()
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);

// BehaviorSubject
const source = interval(1000).pipe(
  take(4),
  publishBehavior(42),
  refCount()
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);

// ReplaySubject
const source = interval(1000).pipe(
  take(4),
  publishReplay(),
  refCount()
);
source.subscribe(observer);
setTimeout(() => source.subscribe(observer), 1000);
setTimeout(() => source.subscribe(observer), 2000);
setTimeout(() => source.subscribe(observer), 4500);
```

##### Controlling Execution with Schedulers
```javascript
import { Observable, of, from, fromEvent, concat, interval, throwError, Subject, asapScheduler, queueScheduler, asyncScheduler, merge
} from 'rxjs';
import { ajax } from 'rxjs/ajax';
import { mergeMap, filter, tap, catchError, take, takeUntil, multicast, refCount, publish, share, publishLast, publishBehavior,   publishReplay, observeOn
} from 'rxjs/operators';
import { allBooks, allReaders } from './data';
import { QueueScheduler } from 'rxjs/internal/scheduler/QueueScheduler';
import { AsapScheduler } from 'rxjs/internal/scheduler/AsapScheduler';

const observer = {
  next: value => console.log(value),
  error: error => console.log(error),
  complete: () => console.log('completed')
};

// Using Schedulers with Observable
// start -> queue -> stop (sync)
// asap -> async (async)
console.log('start');
const queue = of('queue -> ', queueScheduler);
const asap = of('asap', asapScheduler);
const async = of('async', asyncScheduler);
merge(async, asap, queue).subscribe(observer);

// Applying a Scheduler with the observeOn
from([1, 2, 3, 4], queueScheduler)
  .pipe(
    tap(x => console.log(x)),
    observeOn(asyncScheduler),
    tap(x => console.log(x * 2))
  )
  .subscribe(observer);

console.log('stop');
```

##### Testing Your RxJS Code
```javascript
import { TestScheduler } from 'rxjs/testing';
import { expect } from 'chai';
import { delay, take } from 'rxjs/operators';

describe('RxBookTracker Tests', () => {
  let scheduler;
  beforeEach(() => {
    scheduler = new TestScheduler((actual, expected) => {
      expect(actual).deep.equal(expected);
    });
  });

  it('produces a single value and completion message', () => {
    scheduler.run(helpers => {
      const source = helpers.cold('a|');
      const expected = 'a|';
      helpers.expectObservable(source).toBe(expected);
    });
  });

  it('should delay the values produced', () => {
    scheduler.run(helpers => {
      const source = helpers.cold('-a-b-c-d|');
      const expected = '------a-b-c-d|';
      helpers.expectObservable(source.pipe(delay(5))).toBe(expected);
    });
  });

  it('take correct number of values', () => {
    scheduler.run(helpers => {
      const source = helpers.cold('--a--b--c--d|');
      const expected = '--a--b--(c|)';
      const sub =      '^-------!';
      helpers.expectObservable(source.pipe(take(3))).toBe(expected);
      helpers.expectSubscriptions(source.subscriptions).toBe(sub);
    });
  });
});
```
