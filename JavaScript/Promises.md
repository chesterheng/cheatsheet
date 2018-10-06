##### Promises contains set of completed tasks from http://csbin.io/promises

```javascript
// Challenge 1
function sayHello() {
	setTimeout(() => console.log('1. Hello!'), 1000);
}

// Uncomment the line below when ready
sayHello(); // should log "Hello" after 1000ms

// Challenge 2
var promise = new Promise(function (resolve, reject) {
  setTimeout(() => resolve('2. Resolved!') ,1000)
});

// Should print out "Resolved!"
promise.then(value => console.log(value));

// Challenge 3
promise = new Promise(function(resolve, reject) {
  reject('3. Rejected!');
})

// Should print out "Reject!"
promise.catch(value => console.log(value));

// Challenge 4
promise = new Promise(function (resolve, reject) {
  resolve();
});

// Uncomment the lines below when ready
promise.then(() => console.log('4. Promise has been resolved!'));
console.log("4. I'm not the promise!");

// Challenge 5
function delay() {
  return new Promise((resolve, reject) => {
    return setTimeout(() => resolve('5. Hello!'), 1000)
  })
}

sayHello = value => console.log(value);

// Uncomment the code below to test
// This code should log "Hello" after 1000ms
delay().then(sayHello);

// Challenge 6
var secondPromise = new Promise(function (resolve, reject) {
  resolve('6. Second!');
});

var firstPromise = new Promise(function (resolve, reject) {
  resolve(secondPromise);
});

firstPromise.then(value => console.log(value));

// Challenge 7
const fakePeople = [
  { name: 'Rudolph', hasPets: false, currentTemp: 98.6 },
  { name: 'Zebulon', hasPets: true, currentTemp: 22.6 },
  { name: 'Harold', hasPets: true, currentTemp: 98.3 },
]

const fakeAPICall = (i) => {
  const returnTime = Math.floor(Math.random() * 1000);
  return new Promise((resolve, reject) => {
    if (i >= 0 && i < fakePeople.length) {
      setTimeout(() => resolve(fakePeople[i]), returnTime);
    } else {
      reject({ message: "index out of range" });
    }
  });
};

function getAllData() {
  const promises = [fakeAPICall(0), fakeAPICall(1), fakeAPICall(2)]; 
  Promise.all(promises).then(values => console.log(values));
}

getAllData();
```
