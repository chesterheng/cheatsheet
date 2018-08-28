```javascript
const arr = [1, 2, 3, 4];
const result = arr.reduce((cur1, cur2) => cur1 + cur2);
// [ 1, 2 ]
// [ 3, 3 ]
// [ 6, 4 ]
// 1 + 2 + 3 + 4 = 10
```
```javascript
const sym = (...args) => {

  const result = args.reduce((arr1, arr2) => {
    const result = [];
    arr1.forEach(cur => {
      if (arr2.indexOf(cur) < 0 && result.indexOf(cur) < 0) result.push(cur);
    });
    arr2.forEach(cur => {
      if (arr1.indexOf(cur) < 0 && result.indexOf(cur) < 0) result.push(cur);
    });
    return result.sort();
  });

  return result;
}
// arr1 [ 1, 2, 5 ]
// arr2 [ 2, 3, 5 ]
// arr1 [ 1, 3 ]
// arr2 [ 3, 4, 5 ]
sym([1, 2, 5], [2, 3, 5], [3, 4, 5]);
// [ 1, 4, 5 ]
```
