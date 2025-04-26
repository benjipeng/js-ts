# Callbacks

> In JavaScript, functions are `"first-class citizens"`. This means they can be treated like any other variable: they can be assigned to variables, passed as arguments to other functions, and returned from functions.

A **callback function** is just a function that is passed as an argument to another function, with the intention of being "called back" (, or executed) at some later time.

## Understanding Callback Functions in JavaScript

### Basic Callback Example


```javascript
function greet(name, callback) {
  console.log('Hello, ' + name + '!');
  callback();
}

function sayGoodbye() {
  console.log('Goodbye!');
}

// Using the callback
greet('Microsoft Developer', sayGoodbye);
```

When you run this in your console, you'll see:
1. "Hello, Microsoft Developer!"
2. "Goodbye!"

### Callbacks with Parameters

Callbacks can also receive parameters:

```javascript
function processUserInput(name, callback) {
  const capitalizedName = name.toUpperCase();
  callback(capitalizedName);
}

function displayUserName(name) {
  console.log('Your name is: ' + name);
}

// Using the callback with parameters
processUserInput('Alex', displayUserName);
```

### Callbacks for Asynchronous Operations

Callbacks are especially useful for asynchronous operations:

```javascript
function fetchData(url, callback) {
  console.log('Fetching data from: ' + url);
  
  // Simulate network delay with setTimeout
  setTimeout(() => {
    const data = { userId: 1, id: 1, title: 'Sample Data' };
    callback(null, data);
  }, 2000);
}

function handleData(error, data) {
  if (error) {
    console.error('Error fetching data:', error);
    return;
  }
  console.log('Data received:', data);
}

// Using the callback for async operation
fetchData('https://api.example.com/data', handleData);
console.log('Request initiated! Waiting for response...');
```

This demonstrates a common pattern with asynchronous callbacks: the code continues executing while waiting for the callback to be triggered.

**Error Handling with Callbacks:**

The standard pattern for handling errors in callbacks is to pass the error as the first parameter:

```javascript
function divideNumbers(a, b, callback) {
  if (b === 0) {
    callback(new Error('Cannot divide by zero!'), null);
    return;
  }
  
  setTimeout(() => {
    callback(null, a / b);
  }, 1000);
}

function handleResult(error, result) {
  if (error) {
    console.error('Error:', error.message);
    return;
  }
  console.log('Result:', result);
}

// Test success case
divideNumbers(10, 2, handleResult);

// Test error case
divideNumbers(10, 0, handleResult);
```

### Event Listeners as Callbacks

Event listeners are a common use case for callbacks:

```javascript
// Run this in console, then click somewhere on the page
document.addEventListener('click', function(event) {
  console.log('You clicked at position:', event.clientX, event.clientY);
});

console.log('Click listener added! Click anywhere on the page...');
```

### Callback Hell and Solutions

When you have multiple nested callbacks, you can end up with "callback hell":

```javascript
// Example of callback hell - don't write code like this!
function processUserData() {
  fetchUser(function(user) {
    getPermissions(user.id, function(permissions) {
      getPreferences(user.id, function(preferences) {
        updateUI(user, permissions, preferences, function() {
          console.log('UI updated successfully');
        });
      });
    });
  });
}

// Better approach - named functions
function processUserDataBetter() {
  fetchUser(handleUser);
}

function handleUser(user) {
  getPermissions(user.id, function(permissions) {
    handlePermissions(user, permissions);
  });
}

function handlePermissions(user, permissions) {
  getPreferences(user.id, function(preferences) {
    handlePreferences(user, permissions, preferences);
  });
}

function handlePreferences(user, permissions, preferences) {
  updateUI(user, permissions, preferences, function() {
    console.log('UI updated successfully');
  });
}

// Note: These are example functions and won't run directly
```

**Modern Alternatives to Callbacks:**

While callbacks are fundamental, modern JavaScript offers better ways to handle asynchronous operations:

```javascript
// Using Promises
function fetchDataPromise(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const data = { userId: 1, id: 1, title: 'Sample Data' };
      resolve(data);
      // To simulate error: reject(new Error('Network error'));
    }, 2000);
  });
}

// Using the Promise
console.log('Fetching data...');
fetchDataPromise('https://api.example.com/data')
  .then(data => console.log('Data received:', data))
  .catch(error => console.error('Error:', error));
console.log('Request initiated with Promise! Waiting for response...');

// Using async/await (run this in a modern browser)
async function fetchAndProcessData() {
  try {
    console.log('Fetching data with async/await...');
    const data = await fetchDataPromise('https://api.example.com/data');
    console.log('Data received with async/await:', data);
  } catch (error) {
    console.error('Error with async/await:', error);
  }
}

// Call the async function
fetchAndProcessData();
```

**Best Practices for Callbacks:**

1. Keep callback functions small and focused
2. Use meaningful names for callback functions
3. Handle errors in callbacks
4. Avoid deeply nested callbacks
5. Consider using Promises or async/await for complex asynchronous operations
6. Pass the minimum data needed to callbacks
7. Document what your callbacks expect and return

## Practical Example: Data Processing Pipeline

Here's a more complete example you can run in your console:

```javascript
// Data processing pipeline with callbacks
function fetchUserData(userId, callback) {
  console.log(`Fetching data for user ${userId}...`);
  
  // Simulate API call
  setTimeout(() => {
    const userData = {
      id: userId,
      name: 'John Doe',
      email: 'john.doe@example.com',
      role: 'developer'
    };
    callback(null, userData);
  }, 1500);
}

function validateUser(userData, callback) {
  console.log('Validating user data...');
  
  setTimeout(() => {
    if (!userData.email.includes('@')) {
      callback(new Error('Invalid email format'));
      return;
    }
    
    const validatedData = {
      ...userData,
      validated: true,
      validatedAt: new Date().toISOString()
    };
    
    callback(null, validatedData);
  }, 1000);
}

function enrichUserData(userData, callback) {
  console.log('Enriching user data...');
  
  setTimeout(() => {
    const enrichedData = {
      ...userData,
      permissions: ['read', 'write'],
      lastLogin: '2023-11-01T12:00:00Z',
      preferredTheme: 'dark'
    };
    
    callback(null, enrichedData);
  }, 1200);
}

// Execute the pipeline
console.log('Starting user data processing pipeline...');

fetchUserData(123, (error, userData) => {
  if (error) {
    console.error('Error fetching user data:', error);
    return;
  }
  
  console.log('User data fetched:', userData);
  
  validateUser(userData, (error, validatedData) => {
    if (error) {
      console.error('Error validating user data:', error);
      return;
    }
    
    console.log('User data validated:', validatedData);
    
    enrichUserData(validatedData, (error, enrichedData) => {
      if (error) {
        console.error('Error enriching user data:', error);
        return;
      }
      
      console.log('Final processed user data:', enrichedData);
      console.log('Pipeline completed successfully!');
    });
  });
});

console.log('Pipeline initiated! Processing in background...');
```
