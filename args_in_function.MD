1. When you pass a primitive value (such as a number, string, or boolean) as an argument to a function,
   a copy of that value is created and assigned to the function parameter. Any modifications made to
   the parameter within the function will not affect the original value that was passed.

    ```javascript
    function modifyValue(value) {
      value = 10;
    }
    
    let num = 5;
    modifyValue(num);
    console.log(num); // Output: 5
    ```

2. However, when you pass an object (including arrays and functions) as an argument, a copy of the reference
   to that object is passed. This means that both the original and the function parameter will reference the
   same object in memory. If you modify properties of the object within the function, the changes will be
   reflected outside the function as well.

    ```javascript
    function modifyArray(arr) {
      arr.push(4);
    }
    
    let numbers = [1, 2, 3];
    modifyArray(numbers);
    console.log(numbers); // Output: [1, 2, 3, 4]
    ```

3. It's important to note that although the object itself is passed by reference, you cannot reassign
   the entire object to a new value within the function and have that change reflected outside. Only the
   reference itself is passed by reference, not the variable holding the reference.

    ```javascript
    function modifyArray(arr) {
      arr = [4, 5, 6]; // This reassignment won't affect the original array
    }
    
    let numbers = [1, 2, 3];
    modifyArray(numbers);
    console.log(numbers); // Output: [1, 2, 3]
    ```

    - Let's delve into more detail regarding the last point about reassigning the entire
      object within the function.
    - In JavaScript, when you pass an object as an argument to a function, the function parameter
      receives a copy of the reference to the object. This means that both the original variable and
      the parameter point to the same object in memory. Any modifications made to the properties of
      the object through either the original variable or the parameter will affect the underlying object itself.

    - However, reassigning the entire object to a new value within the function will not modify the original
      object or variable outside the function. This is because the assignment operator (=) in JavaScript works
      by assigning a new value to the variable or property, rather than modifying the underlying object.

    - Let's illustrate this with an example:

        ```javascript
        function modifyObject(obj) {
          obj = { prop: 'new value' }; // Reassigning obj to a new object
        }
        
        let originalObj = { prop: 'old value' };
        modifyObject(originalObj);
        console.log(originalObj.prop); // Output: 'old value'
        ```

    - In this example, the `modifyObject` function receives a copy of the reference to the `originalObj`.
    - When we reassign `obj` within the function to a new object `{ prop: 'new value' }`, it does not
      affect the `originalObj` outside the function. The original reference remains intact, and the
      `console.log` statement outputs `'old value'`.

    - This behavior occurs because the assignment `obj = { prop: 'new value' }` creates a new object
      in memory and assigns it to the `obj` parameter. It does not modify the original object or its
      reference. Therefore, the original object remains unchanged.

    - To summarize, when passing objects as arguments in JavaScript functions, modifications to the
      properties of the object will affect the original object. However, reassigning the entire object
      within the function will only affect the function parameter, and the original object or variable
      will remain unaltered.

4. So, in summary, JavaScript passes primitive values by value and objects by reference
   (via a reference to the original object).
