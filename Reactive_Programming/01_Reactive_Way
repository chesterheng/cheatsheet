##### Querying the sequence
```javascript
const registerClicks = event => {
  if(clicks < 10){
    if(event.clientX > window.innerWidth / 2) {
      console.log(event.clientX, event.clientY);
      clicks += 1;
    }
  }
  else {
    document.removeEventListener('click', registerClicks);
  }
};

let clicks = 0;
document.addEventListener('click', registerClicks);
```
```javascript
const observer = {
  next: event => console.log(event.clientX, event.clientY),
  error: error => console.log(error),
  complete: () => console.log('Completed')
}

Rx.Observable
  .fromEvent(document, 'click')
  .filter(event => event.clientX > window.innerWidth / 2)
  .take(10)
  .subscribe(observer);
```

