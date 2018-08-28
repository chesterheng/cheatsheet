```javascript
const sym = (...args) => {
  let result = args[0];

  for(let i = 1; i < args.length; i++){
    result = sym2arr(result, args[i])
  }
  return result;
}

const sym2arr = (arr1, arr2) => {
  const result = [];
  arr1.forEach(cur => {
    if (arr2.indexOf(cur) < 0 && result.indexOf(cur) < 0) result.push(cur);
  });

  arr2.forEach(cur => {
    if (arr1.indexOf(cur) < 0 && result.indexOf(cur) < 0) result.push(cur);
  });
  result.sort();
  return result;
}

sym([1, 2, 5], [2, 3, 5], [3, 4, 5]);
```
