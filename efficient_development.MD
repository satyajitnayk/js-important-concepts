# Efficient Developer Tips:

### 1. Better Reading & Writing URLs in JS

```javascript
const url = `https://builder.io/api/v2/content?model=${model}&locale=${locale}&query.text=${text}`;
const res = await fetch(url);
```

- bug: We are not encoding any of the query param => What someone will generally do:

    ```javascript
    const url = `https://builder.io/api/v2/content?model=${encodeURIComponent(model)}&locale=${encodeURIComponent(locale)}&query.text=${encodeURIComponent(text)}`;
    const res = await fetch(url);
    ```

- but as no of query param increases it become messier:

- One stop Solution

    ```javascript
    const url = new URL(`https://builder.io/api/v2/content`);
    url.searchParams.set('model', model)
    url.searchParams.set('locale', locale)
    url.searchParams.set('text', text)
    const res = await fetch(url);
    ```

### 2. parallel code:

```javascript
const d1 = await asyncProcess1();
const d2 = await asyncProcess2();
```

- we can use promise.all: It returns result of async operations
    ```javascript
    const [d1, d2] = await Promise.all(asyncProcess1, asyncProcess2);
    ```
    - Issue with above code if any of single process fails it will throw error without proceeding further.

- Let's try to use `try & catch` here
    ```javascript
    try {
      const [d1, d2] = await Promise.all(asyncProcess1, asyncProcess2);
    } catch (e) {
      console.log(e)
    }
    ```
    - Issue it doesn't handle error for both async.
- We can do better by using `Promise.allSettled`.
    - It doesn't break if any of the operations fails.
    - It returns:
        - `status`: A string, either "fulfilled" or `rejected`, indicating the eventual state of the promise.
        - `value`: Only present if status is `fulfilled`. The value that the promise was fulfilled with.
        - `reason`: Only present if status is `rejected`. The reason that the promise was rejected with.
    - `Promise.allSettled()` is typically used when you have multiple asynchronous tasks that are not dependent on one
      another
      to complete successfully, or you'd always like to know the result of each promise

        ```javascript
        Promise.allSettled([
          Promise.resolve(33),
          new Promise((resolve) => setTimeout(() => resolve(66), 0)),
          99,
          Promise.reject(new Error("an error")),
        ]).then((values) => console.log(values));
        ```

### 3. Use Map instead of Objects to store key value pairs:

- `Map` perform better in scenarios involving frequent additions & removal of key-value pairs compared to `Objects`.
- Reason being `delete object[key]`  notoriously poor performant.

    ```javascript
    const dataMap = new Map();
    dataMap.set(k, v);
    console.log(dataMap.get(k));
    ```

- Also `Objects` are already cluttered with prefilled keys built into them.

    ```javascript
    const eventsMap = {};
    eventsMap.valueOf
    eventsMap.hasOwnProperty
    eventsMap.isPrototypeOf
    eventsMap.propertyIsEnumerable
    eventsMap.toLocaleString
    ...
    ```

### 4. Swap values with array destructuring

```javascript
let x = 10, y = 20;
console.log(x, y); // print 10 20
[x, y] = [y, x];
console.log(x, y); // print 20 10
```

### 5. Passing arguments as object

- This way of passing arguments has lots of benefits.
- The order of the parameters does not matter anymore, allowing you to concentrate on delivering high-quality code
  instead of repeatedly checking the function definition.
- Auto-completion becomes easier as the IDE will focus on the specific argument that you are providing.
- This method communicates intent in a clear way as function calls specify the value of each property.
- Large codebases will benefit tremendously from the additional verbosity.

```javascript
function addProduct({name, id, price}) {
  // logic
}

addProduct({id, name, price})
```

### 6. Conditionally Add Properties to Object:

- We can use the spread operator, `...`, to quickly add properties to a JavaScript object conditionally.

    ```javascript 
    const condition = true;
    const person = {
      id: 1,
      name: 'John Doe',
      ...(condition && {age: 16}),
    };
    ```

### 7. Object.freeze()

- prevents any modifications to an object, making it immutable.

    ```javascript
    const obj = {name: 'John', age: 30};
    Object.freeze(obj);
    // Now, any attempts to modify the object will have no effect
    obj.name = 'Jane'; // Will not change the value to "Jane"
    ```

### 8. Debouncing & Throttling

- In Node.js, you can implement debouncing and throttling techniques to control the rate at which certain functions are
  executed. These techniques are especially useful for handling events in real-time applications or rate-limited APIs.
- To demonstrate, let's implement debouncing and throttling for a simple function that logs a message with a specified
  delay.

- We'll use the `setTimeout` function to simulate the delay in executing the function.
- `Debouncing in Node.js`:

  ```javascript
  // Debounce function
  function debounce(func, delay) {
    let timerId;
    // This is the inner function that will be returned by the debounce function
    return function (...args) {
    // Before setting a new timer, we clear any existing timer
    clearTimeout(timerId);
        // Set a new timer that will execute the function after the specified delay
        timerId = setTimeout(() => {
          // Call the original function 'func' with the provided arguments
          func.apply(this, args);
        }, delay);
    };
  }
      
  // Function to be debounced
  function logMessage(message) {
    console.log(message);
  }
      
  // Debounced version of the function
  const debouncedLogMessage = debounce(logMessage, 1000);
      
  // Test debouncing
  for(let i=0;i<10;++i) debouncedLogMessage(`Debounced message ${i+1}`);  // Will not log immediately => will log the last message
  ```

    - When the `debouncedLogMessage` function is called multiple times in quick succession, it will only execute the
      last call after the specified delay (1 second). The previous calls will be canceled, and only the most recent
      call
      will take effect.


- `Throttling in Node.js`:

  ```javascript
  // Throttle function
  function throttle(func, limit) {
    // Initialize a variable to keep track of the last time the function was called
     let lastCallTime = 0;
     // Initialize a boolean flag to indicate if the function is currently waiting to execute
     let waiting = false;
     // This is the inner function that will be returned by the throttle function
     return function (...args) {
       // If the function is not currently waiting to execute
       if (!waiting) {
         // Call the original function 'func' with the provided arguments
         func.apply(this, args);
         // Set the waiting flag to true to prevent immediate function execution
         waiting = true;
         // After the specified 'limit' time has passed, reset the waiting flag to false
         // This allows the function to be executed again after the 'limit' time has elapsed
         setTimeout(() => {
           waiting = false;}, limit);
       }
     };
  }
  
  // Function to be throttled
  function logMessageThrottled(message) {
  console.log(message);
  }
      
  // Throttled version of the function
  const throttledLogMessage = throttle(logMessageThrottled, 1000);
      
  // Test throttling
  throttledLogMessage('Throttled message 1'); // Will log immediately
  throttledLogMessage('Throttled message 2'); // Will be ignored
  throttledLogMessage('Throttled message 3'); // Will be ignored
      
  // After 1 second, the next call will be allowed
  setTimeout(() => {
  throttledLogMessage('Throttled message 4'); // Will log after 1 second
  }, 1000);
  ```

    - In this example, the `throttle` function limits the number of times the `logMessageThrottled` function can be
      called
      within a specified time interval (1 second). If the function is called multiple times within the interval,
      only
      the
      first call will be executed, and subsequent calls will be ignored until the interval has passed.

    - These debouncing and throttling techniques can be useful in various scenarios in Node.js, especially when
      dealing
      with
      events or APIs where you want to control the rate of function execution.

### 9. `...args` in function param

- `...args` is a syntax in JavaScript known as the "rest parameter" or "rest syntax." It allows a function to accept any
  number of arguments as an array. The three dots (`...`) before `args` represent the rest parameter, and they indicate
  that the function can handle a variable number of arguments.
- Let's see how it works:

```javascript
function myFunction(...args) {
  console.log(args);
}

myFunction(1, 2, 3, 4);
```

- In this example, we define a function `myFunction` with `...args` as the parameter. When we call `myFunction` with
  arguments `1, 2, 3, 4`, the `args` variable inside the function will capture all these arguments as an array:

Output:

```
[1, 2, 3, 4]
```

- This allows us to handle multiple arguments passed to the function, regardless of the number of arguments provided. We
  can loop through `args` or perform various operations on the elements of the array.

It's important to note that the rest parameter must be the last parameter in the function definition. For example, the
following is not valid:

```javascript
// This will result in a syntax error
function invalidFunction(...args, x) {
  // Function implementation
}
```

- The rest parameter is widely used in modern JavaScript, especially with ES6 features like arrow functions, spread
  syntax, and various utility functions that work with variable-length argument lists.

### 10. TDD(Test-Driven Development)

- It is a software development process where developers write automated tests
  before writing the actual code for the functionality they want to implement.
- The TDD process typically involves the following steps:

    - `Write a Test`: First, you write a test that describes the desired functionality or behavior of the code you want
      to implement. This test will initially fail because the functionality has not been implemented yet.

    - `Run the Test`: Next, you run all the tests you have written so far. Since the new test is expected to fail, you
      should see it fail at this stage.

    - `Implement the Code`: Now, you write the code to fulfill the requirements of the test you just wrote.

    - `Run the Test Again`: After implementing the code, you run all the tests again to ensure the newly written test
      passes and the existing tests continue to pass as well.

    - `Refactor (Optional)`: If needed, you can refactor the code to improve its design or performance without changing
      the behavior. After refactoring, you run the tests again to ensure everything still works correctly.

    - `Repeat`: You repeat the process by writing another test for the next piece of functionality and continue the
      cycle until the entire system is developed.

- The cycle continues as developers work on new features or make changes to existing ones, creating and running tests to
  ensure that everything works as expected and no regressions are introduced.

- TDD has several benefits, including:

    - `Improved code quality`: Writing tests first ensures that the code is thoroughly tested and less prone to bugs and
      defects.

    - `Faster feedback loop`: Developers get immediate feedback on the correctness of their code, which helps catch
      issues early in the development process.

    - `Better design`: TDD often leads to more modular and loosely coupled code since developers need to write testable
      code from the outset.

    - `Documentation:` Tests serve as living documentation, providing examples of how the code should behave and helping
      other developers understand its intended functionality.

    - Confidence in changes: Developers can confidently make changes to the codebase, knowing that if the tests pass,
      the behavior of the code remains unchanged.

- TDD is a popular practice in agile development and is commonly used in conjunction with continuous integration and
  continuous deployment (CI/CD) pipelines to ensure that code changes are thoroughly tested before being deployed to
  production environments.
