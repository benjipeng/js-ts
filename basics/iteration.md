Here is an array:
```javascript
const fruits = ['apple', 'banana', 'cherry', 'date'];
```

1. `for...of` Loop (ES6 - Preferred for iterating value)
```javascript
for (const fruit of fruits){
    console.log(fruit);
}
```
2. `forEach()` Method (Array Method - Preferred for side effects)
> Use it when needed to execute a function for each element primarily for its side effects -> logging to console, updating UI elements, modifying something outside the array.
> `.forEach()` calls the provided callback function once for **each** element in the array. Callback receives `the element's value`, `its index`, and `the array itself`. 
```javascript
fruits.forEach((fruit, index, arr) => {
  // fruit: the current element ('apple')
  // index: the current index (0)
  // arr: the array itself (fruits)
  console.log(`Index ${index}: ${fruit}`);
});
```

While `map`, `filter`, and `reduce` iterate internally, their primary purpose isn't just looping for side effects but creating new values or arrays. They are fundamental to functional programming in JS.
