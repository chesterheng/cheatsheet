##### Callbacks contains set of completed tasks from http://csbin.io/callbacks

```javascript
// Type JavaScript here and click "Run Code" or press Ctrl + s
console.log('Hello, world!');

// Challenge 1
function addTwo(num) {
	return num + 2;
}

// To check if you've completed it, uncomment these console.logs!
console.log(addTwo(3));
console.log(addTwo(10));

// Challenge 2
function addS(word) {
  return `${word}s`;
}

// uncomment these to check your work
console.log(addS('pizza'));
console.log(addS('bagel'));

// Challenge 3
function map(array, callback) {
	let output = [];
	for(let i = 0; i < array.length; i++) {
		output.push(callback(array[i]));
	}
	return output;
}

console.log(map([1, 2, 3], addTwo));

// Challenge 4
function forEach(array, callback) {
	for(let i = 0; i < array.length; i++) {
		callback(array[i]);
	}
}

// see for yourself if your forEach works!
var alphabet = ''
var letters = ['a', 'b', 'c', 'd']
forEach(letters, function (char) {
  alphabet += char
})
console.log(alphabet)

//--------------------------------------------------
// Extension
//--------------------------------------------------

//Extension 1
function mapWith(array, callback) {
	let output = [];
	forEach(array, element => output.push(callback(element)))
	return output;
}

console.log(mapWith([1, 2, 3], addTwo));

//Extension 2
function reduce(array, callback, initialValue) {
	let accumulator;
	if(initialValue === undefined){
		accumulator = array[0];
		array.shift();
	}
	else {
		accumulator = initialValue
	}
	forEach(array, element => accumulator = callback(accumulator, element))
	return accumulator;
}

var nums = [4, 1, 3];
var add = function(a, b) { return a + b; }
console.log(reduce(nums, add, 0));   //-> 8

const intersect = (curr, next) => { 
	const output = [];
	forEach(curr, element => {
		if(next.includes(element)) {
			output.push(element);
		}
	})	
	return output; 
}

//Extension 3
function intersection(...arrays) {
	return reduce(arrays, intersect);
}

console.log(intersection([5, 10, 15, 20], [15, 88, 1, 5, 7], [1, 10, 15, 5, 20]));
// should log: [5, 15]

const mergeUnique = (curr, next) => curr.concat(next.filter(element => curr.indexOf(element) < 0));

//Extension 4
function union(...arrays) {
	return reduce(arrays, mergeUnique);
}

console.log(union([5, 10, 15], [15, 88, 1, 5, 7], [100, 15, 10, 1, 5]));
// should log: [5, 10, 15, 88, 1, 7, 100]

//Extension 5
function objOfMatches(array1, array2, callback) {
	const output = {};
	forEach(array1, element1 => {
		const element2 = array2[array1.indexOf(element1)]
		if(callback(element1) === element2)
			output[element1] = element2;
	});
	return output;
}

console.log(objOfMatches(['hi', 'howdy', 'bye', 'later', 'hello'], ['HI', 'Howdy', 'BYE', 'LATER', 'hello'], function(str) { return str.toUpperCase(); }));
// should log: { hi: 'HI', bye: 'BYE', later: 'LATER' }

//Extension 6
function multiMap(arrVals, arrCallbacks) {
	const output = {};
	forEach(arrVals, element => {
		output[element] = mapWith(arrCallbacks, callback => callback(element));
	});
	return output;
}

console.log(multiMap(['catfood', 'glue', 'beer'], [function(str) { return str.toUpperCase(); }, function(str) { return str[0].toUpperCase() + str.slice(1).toLowerCase(); }, function(str) { return str + str; }]));
// should log: { catfood: ['CATFOOD', 'Catfood', 'catfoodcatfood'], glue: ['GLUE', 'Glue', 'glueglue'], beer: ['BEER', 'Beer', 'beerbeer'] }
```
