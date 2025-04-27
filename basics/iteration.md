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

1. `for...of` Loop

```typescript
console.log("--- for...of (TS) ---");
// Using the mutable array
for (const fruit: string of fruits) {
  console.log(fruit.toUpperCase()); // We know 'fruit' is a string, methods auto-complete
  if (fruit === 'cherry') {
     console.log("Found the cherry!");
     break;
  }
}

console.log("--- for...of (TS with readonly) ---");
// Using the readonly array - prevents accidental modification
for (const fruit: string of readonlyFruits) {
  console.log(fruit.toUpperCase());
   // readonlyFruits.push('extra'); // Error! Property 'push' does not exist on type 'readonly string[]'.
   // readonlyFruits[0] = 'apricot'; // Error! Index signature in type 'readonly string[]' only permits reading.
}
// Output (for the first loop):
// --- for...of (TS) ---
// APPLE
// BANANA
// CHERRY
// Found the cherry!
// Output (for the second loop, without errors uncommented):
// --- for...of (TS with readonly) ---
// APPLE
// BANANA
// CHERRY
// DATE
```

2. `forEach()`
> Type the parameters of the callback function (`fruit: string`, `index: number`). While TypeScript can infer these from the context of fruits.forEach, being explicit is often clearer, especially for complex callbacks.

```typescript
console.log("--- forEach (TS) ---");
readonlyFruits.forEach((fruit: string, index: number) => {
  // Types are checked for fruit (string) and index (number)
  console.log(`Index ${index}: ${fruit.padStart(10, '.')}`); // String methods available
});
// Output:
// --- forEach (TS) ---
// Index 0: .....apple
// Index 1: ...banana
// Index 2: ....cherry
// Index 3: ......date
```

3. Classic `for` Loop

```typescript
console.log("--- Classic for (TS) ---");
for (let i = 0; i < readonlyFruits.length; i++) {
  const fruit: string = readonlyFruits[i]; // Explicit type for the retrieved element
  console.log(`Index ${i}: ${fruit}`);
   // readonlyFruits[i] = 'test'; // Error! Index signature in type 'readonly string[]' only permits reading.
  if (fruit === 'banana') {
    console.log("Skipping banana");
    continue;
  }
   if (i === 2) {
      console.log("Stopping at index 2 (cherry)");
      break;
   }
}
// Output:
// --- Classic for (TS) ---
// Index 0: apple
// Index 1: banana
// Skipping banana
// Index 2: cherry
// Stopping at index 2 (cherry)
```

4. `map()` Method

```typescript
console.log("--- map (TS) ---");
const fruitInfo: string[] = readonlyFruits.map((fruit: string): string => {
  // The `: string` after the parameters declares the callback's return type.
  return `<span class="math-inline">\{fruit\} \(</span>{fruit.length} letters)`; // TS knows fruit is string
});
console.log("Original:", readonlyFruits);
console.log("Mapped:", fruitInfo);
// Output:
// --- map (TS) ---
// Original: [ 'apple', 'banana', 'cherry', 'date' ]
// Mapped: [ 'apple (5 letters)', 'banana (6 letters)', 'cherry (6 letters)', 'date (4 letters)' ]
```

5. `filter()` Method

```typescript
console.log("--- filter (TS) ---");
const longFruits: string[] = readonlyFruits.filter((fruit: string): boolean => {
   // Callback must return boolean
   return fruit.length > 5;
});
console.log("Original:", readonlyFruits);
console.log("Filtered (length > 5):", longFruits);
// Output:
// --- filter (TS) ---
// Original: [ 'apple', 'banana', 'cherry', 'date' ]
// Filtered (length > 5): [ 'banana', 'cherry' ]
```