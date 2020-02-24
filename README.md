# Advanced JavaScript Old Readme


* _Note_ 
  * The Portfolio portion of job prep should be taught this afternoon
  * It can be found in `/job-prep/10.5-portfolios
  * It typically takes 30 minutes
  * It should be taught immediately following lunch
  * Students can begin working on their portfolio immediately

## Status

The repl.it tests are almost done for the errors section, but not for the rest. Here's the classroom link. [https://repl.it/teacher/classrooms/109341](https://repl.it/teacher/classrooms/109341)

## Objectives

* Errors
  * Student can throw errors
  * Student can use try/catch to catch errors
  * Student can display an appropriate error message to user
  * Student can re-throw
  * Student can use nested try/catch
  * Student can use finally to clean up after try/catch
* Async
  * Student can explain JavaScript's event loop
    * Instructors: refer to video linked in lecture
  * Student can use node-style callbacks, including error handling
  * Student can explain why try/catch does not work with node-style callbacks
* Async/await
  * Student can use async/await
  * Student can explain that async functions always return promises
* Promises
  * Student can explain why promises have become popular (contrast to callbacks)
  * Student can use promise chaining
  * Student can turn callback into promise
  * Student can chain a .then to a .catch as an async equivalent of `finally`
* Memoization
  * Student understands what memoization is and why it is useful
  * Student can create a function using the memoization technique
* Hoisting
  * Student can explain what hoisting is
  * Student can explain the difference between a function declaration vs function expression and how it applies to hoisting
  * Student understands the difference between declaring a variable with var, let, and const and how it applies to hoisting


## Resources

* [Event Loop Visualization](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
 * can be useful in the Promises & async await section
* [Memoization Examples](https://github.com/steven-isbell/memoization)

## Lesson

This lecture teaches concepts in isolation, and then there is a mini project to do.

**Concepts In Isolation**

* Errors
  * Reminder: we've already of dealt with errors by using `.catch()` when we did database queries with massive.
  * Why throw errors? Why not just return (e.g.) -1, like the `indexOf` function?
    * Doesn't always make sense
    * What if your job is to write a function that takes a single parameter and return it. (This is known as the identity function.) What happens if no parameter is supplied at all? You can't e.g. return `undefined`. There's no way for the caller of your function to know that what you're returning is what was passed in or an error.
  * Throw
    * Here's how to throw an error.
    * Show. Write identity function, returning what was passed in. Throw 'No parameter was supplied' if nothing was passed.
      * Teach the concept of `arguments`. Must use regular function for `arguments`, not arrow function.
    * Repl.it test: Write function `addOne` that takes a parameter. Throw 'Parameter ${val} must be numeric' if it's not a number. Otherwise, return the number plus one.
      * https://repl.it/teacher/assignments/2315001/edit
  * Try/catch
    * If a function you're using throws an error, what can you do about it? You can catch it.
    * Show. Write a function called `invertValue` which will call an existing function `getValue`. Your function should invert the value (use the bang operator) and return it. If `getValue` throws, catch it and alert the user to the problem, in this case just `console.log` the message `The system is down. Please try again later.`.
    * Repl.it test. Write a function called `addValues` which will call existing functions `getValue1` and `getValue2` and add the values and return the sum. If either `getValue1` or `getValue2` throws, catch it and alert the user to the problem, in this case just `console.log` the message "The system is down. Please try again later.".
      * https://repl.it/teacher/assignments/2317544/edit
  * Re-throw.
    * It's possible to observe the error, and handle it, or rethrow it, forcing someone else higher up the chain to deal with it.
    * Repl.it test. Write a function getValueOrCache that that takes two parameters, getValue and getCacheValue. Call getValue and return its value. If it throws an error, you should catch it. If the error message is "Can't connect to internet", call getCacheValue and return its value. If it's another error, re-throw it.
      * https://repl.it/teacher/assignments/2318115/edit
  * Nested try/catch
    * Write functions `a`, `b`, and `c`, where `a` calls `b` which calls `c`. `c` should throw, and be caught in `a`. The lesson is that when an error is thrown, the stack is "unwound" until it finds a try/catch. So `b` is checked, and since there is no `try/catch`, then it goes down the stack to `a`.
      * This is called "unwinding the stack".
      * Show how even if there is other code in `b` (other than `try/catch`), that doesn't matter.
      * If there is no try/catch at all, what happens depends on the javascript environment. In a browser, you'll see the error in the console, and the app will keep running (although probably not in a "good" state).
      * In node, the server will die. See `server/server-dies-example.js`.
      * Then show adding a try/catch in `b`. In the catch, console.log the error and show how we can re-throw or throw a different error.
    * No repl.it test needed.
  * Finally block
    * The finally block is a way to DRY up our code. If we have code that always needs to run whether an error occurs or not, rather than put that code in both the try block and catch block, we can put it just in the finally block.
    * Show:
      ```javascript
      let loading = false;
      function load() {
        loading = true;
        try {
          someOtherFunctionThatThrows();
          // Don't need to do this here:
          // loading = false;
        } catch {
          // Some cleanup would go on this line.
          // And no need to do this (either):
          // loading = false;
        } finally {
          loading = false;
        }
      }
      ```
    * Repl.it test. Need to write!

* Discuss async
  * Show an axios call
  * Now put console.log(1) above it, console.log(2) inside the axios callback, and console.log(3) after it all. Show students how it goes 1, 3, 2. Why?
  * Show how it's the same for callbacks (as it is for promises, it's still 1, 3, 2) with `setTimeout()`
  * Discuss node-style callbacks
    * Show them node's `fs.readFile()` to show "node-style" callbacks (with `err` as first parameter)
  * Show them event loop video: https://www.youtube.com/watch?v=8aGhZQkoFbQ&vl=en
    * Watch until ~17:30
  * Promises
    * `nodemon server/promiseDemo.js`
    * Remind with a promise we can assign it to a variable. It's like the number from Chick-fil-A.
    * Show resolve
    * Show reject
    * .then should return a promise
    * why promises are a better solution and have become so popular
      * callback hell
      * show how nested `.then()`'s aren't any better than callback hell though
      * promises are more composable
    * can chain a .then to a .catch, e.g. when I do `.catch(...).then(() => this.setState({ isLoading: false }))`
      * We can chain instead of nesting!
    * We can turn any function with a callback into a promise
      * E.g. turn setTimeout into a `delay(1000).then(...)`
  * Async/await

* Discuss Memoization
  * memoization is a programming technique that is meant to increase a functions performance by caching is previously computed results.
  * here's an example of an memoized add function:
  ``` javascript
  const memoizedAdd = () => {
    let cache = {};
    return (value) => {
      if (value in cache) {
        console.log('Fetching from cache');
        return cache[value];
      } else {
        console.log('Calculating result');
        let result = value + 10;
        cache[value] = result;
        return result;
      }
    }
  } 
  ```

* Discuss Hoisting
  * in the example below we can see that shape is available before it is declared.  It hasn't been given it's value yet though.  The declaration is brought to the top of the local or global scope because of hoisting.

  ```javascript
  console.log(1111111, shape)

  var shape = 'round'

  console.log(22222222, shape)
  ```

  * we can see here that when we declare a varibale with `let` it is not hoisted

  ```javascript
  console.log(11111111, shape)

  let shape = 'round'

  console.log(222222222, shape)
  ``` 

  * with a function declaration, we can invoke a function before it has been declared because hoisting will bring the funtion to the top of the scope it was declared in.  This will not work with a function expression
  
  ```javascript

  sayHi()
  
  function sayHi() {
    console.log('hello there!')
  }
  ```


## Afternoon project

* Portfolio
