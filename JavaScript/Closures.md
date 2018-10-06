##### Closures contains set of completed tasks from http://csbin.io/closures

```javascript
// Type JavaScript here and click "Run Code" or press Ctrl + s
console.log('Hello, world!');

function createFunction() {
	return () => console.log('hello');
}

// UNCOMMENT THESE TO TEST YOUR WORK!
var function1 = createFunction();
function1();

function createFunctionPrinter(input) {
	return () => console.log(input);
}

// UNCOMMENT THESE TO TEST YOUR WORK!
var printSample = createFunctionPrinter('sample');
printSample();
var printHello = createFunctionPrinter('hello');
printHello();

function outer() {
  var counter = 0; // this variable is outside incrementCounter's scope
  function incrementCounter () {
    counter ++;
    console.log('counter', counter);
  }
  return incrementCounter;
}

var willCounter = outer();
var jasCounter = outer();

// Uncomment each of these lines one by one.
// Before your do, guess what will be logged from each function call.

willCounter(); // counter 1
willCounter(); // counter 2
willCounter(); // counter 3
jasCounter();  // counter 1
willCounter(); // counter 4

function addByX(x) {
	return (y) => y + x; 
}

var addByTwo = addByX(2);
console.log(addByTwo(1)); //should return 3
console.log(addByTwo(2)); //should return 4
console.log(addByTwo(3)); //should return 5

var addByThree = addByX(3);
console.log(addByThree(1)); //should return 4
console.log(addByThree(2)); //should return 5

var addByFour = addByX(4);
console.log(addByFour(4)); //should return 8
console.log(addByFour(10)); //should return 14

//--------------------------------------------------
// Extension
//--------------------------------------------------

function once(func) {
  let firstTime = true;
  let result;
  
  return function(num) {
    if (firstTime) {
      firstTime = false;
      result = func(num);
      return result
    }
    return result;
  }
}

var onceFunc = once(addByTwo);

// UNCOMMENT THESE TO TEST YOUR WORK!
console.log(onceFunc(4));  //should log 6
console.log(onceFunc(10));  //should log 6
console.log(onceFunc(9001));  //should log 6

function after(count, func) {
	let counter = 1;
	return function(){
    if(counter === count) {
      func();
    }
    counter++;
  }
}

var called = function() { console.log('hello') };
var afterCalled = after(3, called);

afterCalled(); // -> nothing is printed
afterCalled(); // -> nothing is printed
afterCalled(); // -> 'hello' is printed

function delay(func, wait) {
	setTimeout(() => func(), wait);
}

delay(() => console.log("Hi"), 1000);
```
