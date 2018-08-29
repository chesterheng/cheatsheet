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
 
  newInv.forEach(new_item => {
    // new_item exist in curInv?
    const index = curInv.findIndex(cur_item => cur_item[1] === new_item[1]);

    if (index > -1) {
      // update item quantity in curInv
      curInv[index][0] += new_item[0];
    }
    else {
      // add new_item to curInv
      curInv.push(new_item);
    }
  });
  
  // sort curInv by name
  curInv.sort((a, b) => {
    const nameA = a[1].toUpperCase(); // ignore upper and lowercase
    const nameB = b[1].toUpperCase(); // ignore upper and lowercase
    if (nameA < nameB) {
      return -1;
    }
    if (nameA > nameB) {
      return 1;
    }
    // names must be equal
    return 0;
  });
  return curInv;
}

// Example inventory lists
var curInv = [
    [21, "Bowling Ball"],
    [2, "Dirty Sock"],
    [1, "Hair Pin"],
    [5, "Microphone"]
];

var newInv = [
    [2, "Hair Pin"],
    [3, "Half-Eaten Apple"],
    [67, "Bowling Ball"],
    [7, "Toothpaste"]
];

updateInventory(curInv, newInv);
```
