```
curInv = [
[21, "Bowling Ball"], 
[2, "Dirty Sock"], 
[1, "Hair Pin"], 
[5, "Microphone"]]

newInv = [[2, "Hair Pin"], 
[3, "Half-Eaten Apple"], 
[67, "Bowling Ball"], 
[7, "Toothpaste"]]

result = [[88, "Bowling Ball"], 
[2, "Dirty Sock"], 
[3, "Hair Pin"], 
[3, "Half-Eaten Apple"], 
[5, "Microphone"], 
[7, "Toothpaste"]]
```
```javascript
const updateInventory = (curInv, newInv) => {
  const result = [...curInv];
  newInv.forEach(new_item => {
    const cur_item = curInv.find(cur_item => cur_item[1] === new_item[1]);

    if (cur_item) {
      // update cur_item quantity
      cur_item[0] += new_item[0];
    }
    else {
      // add new_item
      result.push(new_item);
    }
  });

  result.sort((a, b) => {
    const nameA = a[1].toUpperCase(); // ignore upper and lowercase
    const nameB = b[1].toUpperCase(); // ignore upper and lowercase
    if (nameA < nameB) {
      return -1;
    }
    else if (nameA > nameB) {
      return 1;
    }
    // names must be equal
    else return 0;
  });
  console.log(result);
  return result;
}

// Example inventory lists
const curInv = [
  [21, "Bowling Ball"],
  [2, "Dirty Sock"],
  [1, "Hair Pin"],
  [5, "Microphone"]
];

const newInv = [
  [2, "Hair Pin"],
  [3, "Half-Eaten Apple"],
  [67, "Bowling Ball"],
  [7, "Toothpaste"]
];

updateInventory(curInv, newInv);
/*
result = [ [ 88, 'Bowling Ball' ],
  [ 2, 'Dirty Sock' ],
  [ 3, 'Hair Pin' ],
  [ 3, 'Half-Eaten Apple' ],
  [ 5, 'Microphone' ],
  [ 7, 'Toothpaste' ] ]
*/
```
