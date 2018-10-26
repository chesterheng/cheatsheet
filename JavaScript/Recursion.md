http://csbin.io/recursion

```javascript
// Type JavaScript here and click "Run Code" or press Ctrl + s
console.log('Hello, world!');


// Challenge 1
function countdown(n) {
	if(n === 0) return;
  console.log(n);
  return countdown(n - 1);
}

// To check if you've completed it, uncomment these console.logs!
//countdown(5);
//countdown(10);


// Challenge 2
function sum(array, total = 0) {
	if(string.length === 0) return true;
  return sum(array.slice(1), total + array[0]);
  //[1,2,3].slice(1) -> [2,3]
}

// uncomment these to check your work
//console.log(sum([1,1,1])); // -> returns 3
//console.log(sum([1,2,3,4,5,6])); // -> returns 21

// Challenge 3
function palindrome(string) {
  if(string.length === 0 || string.length === 1) return true;
  const array = string.toLowerCase().replace(/[^a-z]/g, "").split('');
  const first = array.shift();
  const last = array.pop();
  return (first === last) && palindrome(array.join(''));
}

console.log(palindrome("Anne, I vote more cars race Rome-to-Vienna")); //-> true
console.log(palindrome("llama mall")); //-> true
console.log(palindrome("jmoney")); //-> false


// Challenge 4
// square root (337) Prime numbers less than 19 are 2, 3, 5, 7, 11, 13, 17 

function isPrime(num) {
	if(num === 1) return false;
  
}

//console.log(isPrime(1)); //-> false
//console.log(isPrime(2)); //-> true
//console.log(isPrime(3)); //-> true
//console.log(isPrime(4)); //-> false


//--------------------------------------------------
// Extension
//--------------------------------------------------
//Extension 1
function pathFinder(obj, arr) {

}

// const obj = { first: { second: { third: "finish" } }, second: { third: "wrong" } };
// const arr = ["first", "second", "third"];
// console.log(pathFinder(obj, arr));   //-> "finish"


//Extension 2
function flattenRecursively(arr) {

}

// console.log(flattenRecursively([1, [2, 3, [4]]])); //-> [1, 2, 3, 4]
// console.log(flattenRecursively([1, {}, [3, [[4]]]])); //-> [1, {}, 3, 4]


//Extension 3
function findInOrderedSet(arr, target) {

}

// const nums = [1, 4, 6, 7, 9, 17, 45];
// console.log(findInOrderedSet(nums, 4));  //-> true
// console.log(findInOrderedSet(nums, 2));  //-> false
```
