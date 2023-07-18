**In JavaScript, arguments in functions are passed by value.**

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

4. So, in summary, JavaScript passes primitive values by value and objects by reference
   (via a reference to the original object).
