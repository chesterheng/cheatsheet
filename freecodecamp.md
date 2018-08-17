##### Cpmpleted freeCodeCamp Projects

###### APIs and Microservices Projects
* Timestamp Microservice
    * Demo: https://fcc-tsms.glitch.me
    * Code: https://github.com/chesterheng/fcc-tsms
* Request Header Parser Microservice
    * Demo: https://fcc-rhpms.glitch.me
    * Code: https://github.com/chesterheng/fcc-rhpms
* URL Shortener Microservice
    * Demo: https://fcc-sms.glitch.me
    * Code: https://github.com/chesterheng/fcc-sms
* Exercise Tracker
    * Demo: https://fcc-et.glitch.me
    * Code: https://github.com/chesterheng/fcc-et
* File Metadata Microservice
    * Demo: https://fcc-et.glitch.me
    * Code: https://github.com/chesterheng/fcc-fmdms
    
###### JavaScript Algorithms and Data Structures Projects
* Palindrome Checker
```javascript
function palindrome(str) {
  // remove all non-alphanumeric characters (punctuation, spaces and symbols) 
  // turn everything into the same lower 
  str = str.replace(/[\W_]/g,'').toLowerCase();

  // reverse the given string
  const strRev = str.split("").reverse().join("");

  // check for Palindrome 
  return str === strRev ? true : false;
}
```
* Roman Numeral Converter
```javascript
function convertToRoman(num) {
    const romObj = {1000: 'M', 900: 'CM', 500: 'D', 400: 'CD' ,100: 'C', 90: 'XC', 50: 'L', 40: 'XL', 10: 'X', 9:'IX', 5: 'V', 4: 'IV', 1: 'I'}
    const romVal = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1];

    // dividend / divisor = quotient and remainder
    const romArr = [];
    let dividend = num;
    let divisor, remainder, quotient; 

    while(true){
        // look for divisor
        divisor = romVal.find(val => dividend >= val);
        // 36 / 10 = 3
        quotient = Math.floor(dividend / divisor);
        // push XXX
        for(let i=0; i<quotient; i++){
            romArr.push(romObj[divisor]);
        }
        // 36 / 10 r = 6
        remainder = dividend % divisor;

        if(remainder === 0) break;
        else dividend = remainder;
    }
    return romArr.join('');
   }
```
* Caesars Cipher
```javascript
function rot13(str) { // LBH QVQ VG!
    return str.split('').map(x => {
        if(/[A-M]/.test(x))         return String.fromCharCode(x.charCodeAt(0) + 13);
        else if(/[N-Z]/.test(x))    return String.fromCharCode(x.charCodeAt(0) - 13);
        else                        return x;
      }).join('');
  }
```
* Telephone Number Validator
```javascript
function telephoneCheck(str) {
   // 1st group: "1" or "1 " -> (1\s?)?
   // 2nd group: "(XXX)" or "XXX" -> (\(\d{3}\)|\d{3})
   // optional seperator: " " or "-" -> [\s\-]?
   // 3rd group: "XXX" -> \d{3}
   // optional seperator: " " or "-" -> [\s\-]?
   // 4th group: "XXXX" -> \d{4}
   return /^(1\s?)?(\(\d{3}\)|\d{3})[\s\-]?\d{3}[\s\-]?\d{4}$/.test(str);
}
```
* Cash Register
```javascript
function checkCashRegister(price, cash, cid) {

  let change = cash - price;
  const cidValue = cid.reduce((acc, cur) => acc + cur[1], 0);
  
  // cash-in-drawer is less than the change due
  if(cidValue < change)         return {status: "INSUFFICIENT_FUNDS", change: []};
  //the value for the key change if it is equal to the change due
  else if(cidValue === change)  return {status: "CLOSED", change: cid};

  const unitkeys = [100, 20, 10, 5, 1, 0.25, 0.1, 0.05, 0.01];
  // sorted in highest to lowest order
  const cidValues = cid.reverse();
  const changeArr = [];

  let tempCount, tempValue;
  let changeCount, valueCount;
  for(let i=0; i< cidValues.length; i++){
    
    if(change >= unitkeys[i] && cidValues[i][1] !== 0){
      changeCount = Math.floor(change / unitkeys[i]);
      valueCount = Math.floor(cidValues[i][1] / unitkeys[i]);

      tempCount = changeCount > valueCount ? valueCount : changeCount;
      tempValue = tempCount * unitkeys[i];

      changeArr.push([cidValues[i][0], tempValue]);
      change -= tempValue;
      change = Number.parseFloat(change).toFixed(2);

      if(change === 0) break;
    }
  }

  // cannot return the exact change
  if(change > 0) return {status: "INSUFFICIENT_FUNDS", change: []};
  else return {status: "OPEN", change: changeArr};  
}
```

###### Responsive Web Design Projects
* Build A Tribute Page
    * Demo: https://tp.surge.sh
    * Code: https://github.com/chesterheng/tribute-page
* Build A Survey Form
    * Demo: https://dsf.surge.sh
    * Source code: https://github.com/chesterheng/survey-form
* Build A Product Landing Page
    * Demo: https://pl.surge.sh
    * Source code: https://github.com/chesterheng/product-landing
    
###### Front End Libraries Projects

###### Data Visualization Projects

###### Information Security and Quality Assurance Projects
