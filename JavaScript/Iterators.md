##### Promises contains set of completed tasks from http://csbin.io/iterators

```javascript
// Type JavaScript here and click "Run Code" or press Ctrl + s
console.log('Hello, world!');

// CHALLENGE 1
function sumFunc(arr) {
  let sum = 0;
  for(let i = 0; i < arr.length; i++){
    sum += arr[i];
  }
	return sum;
}

// Uncomment the lines below to test your work
const array = [1, 2, 3, 4];
console.log(sumFunc(array)); // -> should log 10

function returnIterator(arr) {
  let i = 0;
  function inner() {
    const element = arr[i];
    i++;
    return element;
  }
	return inner;
}

// Uncomment the lines below to test your work
const array2 = ['a', 'b', 'c', 'd'];
const myIterator = returnIterator(array2);
console.log(myIterator()); // -> should log 'a'
console.log(myIterator()); // -> should log 'b'
console.log(myIterator()); // -> should log 'c'
console.log(myIterator()); // -> should log 'd'

// CHALLENGE 2
function nextIterator(arr) {
  let i = 0;
  const inner = {
    next: function () {
      const element = arr[i];
      i++;
      return element;
    }
  }
	return inner;
}

// Uncomment the lines below to test your work
const array3 = [1, 2, 3];
const iteratorWithNext = nextIterator(array3);
console.log(iteratorWithNext.next()); // -> should log 1
console.log(iteratorWithNext.next()); // -> should log 2
console.log(iteratorWithNext.next()); // -> should log 3

// CHALLENGE 3
function sumArray(arr) {
  let sum = 0;
  const iterator = nextIterator(arr);
  for(let i = 0; i < arr.length; i++){
    sum += iterator.next();
  }
  return sum;
}

// Uncomment the lines below to test your work
const array4 = [1, 2, 3, 4];
console.log(sumArray(array4)); // -> should log 10

// CHALLENGE 4
function setIterator(set) {
  let iterator = set.values();
  return {
    next: function () {
      return iterator.next().value;
    }
  }
}

// Uncomment the lines below to test your work
const mySet = new Set('hey');
const iterateSet = setIterator(mySet);
console.log(iterateSet.next()); // -> should log 'h'
console.log(iterateSet.next()); // -> should log 'e'
console.log(iterateSet.next()); // -> should log 'y'

// CHALLENGE 5
function indexIterator(arr) {
  let i = 0;
  const inner = {
    next: function () {
      const element = [i, arr[i]]
      i++;
      return element;
    }
  }
	return inner;
}

// Uncomment the lines below to test your work
const array5 = ['a', 'b', 'c', 'd'];
const iteratorWithIndex = indexIterator(array5);
console.log(iteratorWithIndex.next()); // -> should log [0, 'a']
console.log(iteratorWithIndex.next()); // -> should log [1, 'b']
console.log(iteratorWithIndex.next()); // -> should log [2, 'c']

// CHALLENGE 6
function Words(string) {
  this.str = string;
  return this.str.split(" ");
}

Words.prototype[Symbol.iterator] = function() {
  let i = 0
  const inner = {
    next: function () {
      const element = this[i];
      i++;
      return element;
    }
  }
  return inner;
}

// Uncomment the lines below to test your work
const helloWorld = new Words('Hello World');
for (let word of helloWorld) { console.log(word); } // -> should log 'Hello' and 'World'

// CHALLENGE 7
function valueAndPrevIndex(array){
	let i = 0;
  const inner = {
    sentence: function () {
      const element = i === 0 ? 
            `${array[i]} is the first element` : 
						`${array[i]} was found after index ${i - 1}`;
      i++;
      return element;
    }
  }
  return inner;
}

const returnedSentence = valueAndPrevIndex([4,5,6]);
console.log(returnedSentence.sentence()); // '4 is the first element'
console.log(returnedSentence.sentence()); // '5 was found after index 0'
console.log(returnedSentence.sentence()); // '6 was found after index 1'

//CHALLENGE 8
function* createConversation(string) {
	yield setInterval(function() {
		console.log(string === "english" ? "hello there" : "gibberish");
  }, 3000)
}
console.log(createConversation('english').next());

//CHALLENGE 9
function waitForVerb(noun) {
	const verb = "bark";
	const promise = new Promise((resolve, reject) => {
		setTimeout(() => resolve(noun + " " + verb), 3000);
	});
	return promise;
}

async function f(noun) {
	const data = await waitForVerb(noun);
	console.log(data);
}

f("dog");
```
