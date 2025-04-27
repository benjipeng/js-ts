# Making iterations
## Javascript
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

3. Classic `for` loop (index-based)

```javascript
for (let i = 0; i < fruits.length; i++) {
    const fruit = fruits[i];
    console.log(`Index: ${i}, Fruit: ${fruit}`);
}
```

While `map`, `filter`, and `reduce` iterate internally, their primary purpose isn't just looping for side effects but creating new values or arrays. -> Fundamental to functional programming in JS.

4. `map()` (Creating a new array by transforming elements)

```javascript
const fruitLengths = fruits.map((fruit, index) => {
  // Returns a new value for each element
  return `<span class="math-inline">\{fruit\} \(</span>{fruit.length} letters)`;
});
console.log("Original:", fruits);
console.log("Mapped:", fruitLengths);
```

5. `filter()` (Creating a new array with a subset of elements)

```javascript
const longFruits = fruits.filter((fruit, index) => {
   // Return true to keep the element, false to discard it
   return fruit.length > 5;
});
console.log("Original:", fruits);
console.log("Filtered (length > 5):", longFruits);
```

## Typescript

> `noImplicitAny`: It's a standard practice to enable the noImplicitAny compiler option in `tsconfig.json`. This forces user to be explicit about types where TypeScript can't infer them, preventing potential runtime errors.

> `readonly`: Using `readonly` types signal the intent NOT to modify the array during iteration and prevents accidental mutations.

```ts
const fruits: string[] = ['apple', 'banana', 'cherry', 'date'];

// Or, for immutability (best practice if you don't intend to change the array):
const readonlyFruits: readonly string[] = ['apple', 'banana', 'cherry', 'date'];
// or ReadonlyArray<string>
```