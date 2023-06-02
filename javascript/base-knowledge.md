# JS base cheat<!-- omit in toc -->

## Table of Contents<!-- omit in toc -->

- [1) Explain Hoisting](#1-explain-hoisting)
- [2) Explain the Event Loop](#2-explain-the-event-loop)
- [3) Explain Event Bubbling](#3-explain-event-bubbling)
- [4) Explain prototype](#4-explain-prototype)
- [5) What is a pure function](#5-what-is-a-pure-function)
- [6) How do you get the type of an object](#6-how-do-you-get-the-type-of-an-object)
- [7) When do you use: var, let or const?](#7-when-do-you-use-var-let-or-const)
- [8) How many execution threads can be ran or a usually ran in Javascript?](#8-how-many-execution-threads-can-be-ran-or-a-usually-ran-in-javascript)
- [9) Can you explain closures and provide an example?](#9-can-you-explain-closures-and-provide-an-example)
- [10) Explain the Module pattern](#10-explain-the-module-pattern)
- [11) Can you explain how `this` keyword works in JavaScript?](#11-can-you-explain-how-this-keyword-works-in-javascript)
- [12) What's the difference between `==` and `===`?](#12-whats-the-difference-between--and-)
- [13) Can you explain the difference between `null` and `undefined`?](#13-can-you-explain-the-difference-between-null-and-undefined)
- [14) What is a Promise? How would you handle multiple asynchronous operations with Promises?](#14-what-is-a-promise-how-would-you-handle-multiple-asynchronous-operations-with-promises)
- [15) Can you explain how async/await works? And what is the difference with Promise?](#15-can-you-explain-how-asyncawait-works-and-what-is-the-difference-with-promise)
- [16) What are callbacks and what issues might arise when using them?](#16-what-are-callbacks-and-what-issues-might-arise-when-using-them)
- [17) What are higher-order functions?](#17-what-are-higher-order-functions)
- [18) What are classes in JavaScript and how do they work?](#18-what-are-classes-in-javascript-and-how-do-they-work)
- [19) How would you improve the performance of a JavaScript web application?](#19-how-would-you-improve-the-performance-of-a-javascript-web-application)
- [20) Can you discuss ways to profile and debug JavaScript code?](#20-can-you-discuss-ways-to-profile-and-debug-javascript-code)
- [21) What new features were introduced in ES6 (ES2015) and later versions that you find particularly useful?](#21-what-new-features-were-introduced-in-es6-es2015-and-later-versions-that-you-find-particularly-useful)
- [22) Can you explain destructuring and how you would use it?](#22-can-you-explain-destructuring-and-how-you-would-use-it)
- [ES6](#es6)

# 1) Explain Hoisting

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their containing scope during the compile phase, before the code has been executed.

This means that you can use variables and functions before they have been declared. Here's an example:

```javascript
console.log(myVar); // undefined, not ReferenceError
var myVar = 5;
```

In the example above, you might expect a ReferenceError because `myVar` is logged before it's defined. But due to hoisting, JavaScript has already set up the variable at the top of the scope with a default value of `undefined`.

However, it's important to note that only the declarations are hoisted, not the initializations.

Here's another example but with functions:

```javascript
console.log(myFunction()); // "Hello"

function myFunction() {
  return "Hello";
}
```

In this case, the function declaration (and its body) is hoisted, so it's fully usable before it's declared.

It's also important to note that the hoisting behavior is different with `let` and `const` compared to `var`.

```javascript
console.log(myLetVar); // ReferenceError
let myLetVar = 5;
```

While `var` declarations are initialized with `undefined` and hoisted to the top, `let` and `const` declarations are not initialized and they're hoisted to the top of the block (not the function or the global scope), but accessing them before declaration results in a Reference Error.

This can lead to a temporal dead zone for `let` and `const` variables, a time span between variable creation and initialization when accessing the variable results in an error.

In practice, it's generally a good idea to declare and initialize your variables at the top of their scope to avoid confusion related to hoisting.

# 2) Explain the Event Loop

The JavaScript Event Loop is a key part of how JavaScript works as a language. It's responsible for executing code, collecting and processing events, and executing queued sub-tasks.

Here's a simplified explanation of how the Event Loop works:

1. **Stack**: JavaScript initially executes all code on a single main thread, and this execution context is known as the Execution Stack or Call Stack. When a function is called, it's added to the call stack. If that function calls another function, that function is added on top of the call stack. This continues until there are no more functions to add to the stack (i.e., all functions have returned).

2. **Heap**: Objects are allocated in a heap which is just a name to denote a large region of memory.

3. **Queue**: In addition to the call stack, there's a structure known as the Task Queue (or Event Queue). When an asynchronous event occurs (like a user clicking a button, a timer firing, or an API data return), a message is added to the task queue.

4. **Event Loop**: The Event Loop's job is to look at the call stack and the task queue. If the call stack is empty and there are tasks in the task queue, it will dequeue a message (remove from the front of the queue) and the corresponding callback function will be added to the call stack (and thereby start executing).

This process continues indefinitely for as long as the application is running. This is why it's called the "Event Loop". It's a loop that continually processes events.

The beauty of this model is that JavaScript, although single-threaded, can handle many tasks asynchronously via the event loop and task queue.

This event-driven, non-blocking I/O model makes JavaScript particularly suitable for tasks that primarily consist of many I/O operations, like web servers, real-time services, and certain kinds of microservices. This is one of the reasons Node.js (which uses the same V8 JavaScript engine and thus the same event loop model) is popular for backend development.

# 3) Explain Event Bubbling

Event bubbling is a type of event propagation in JavaScript where an event not only runs on the element that was directly targeted by that event (like a click, keypress, etc.), but also on all of its parent elements, going all the way up to the root of the document.

When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

Here's an example:

```html
<div id="parent">
  Parent
  <button id="child">Child</button>
</div>

<script>
  document.querySelector("#child").addEventListener("click", () => {
    alert("Child clicked!");
  });

  document.querySelector("#parent").addEventListener("click", () => {
    alert("Parent clicked!");
  });
</script>
```

In this example, if you click on the "Child" button, you'll first see an alert saying "Child clicked!", and then another alert saying "Parent clicked!". This is because the click event is first handled by the button, and then it bubbles up to the parent div element.

Event bubbling is the default method of event propagation in JavaScript, but it's not the only one. There's also event capturing, which is the opposite of event bubbling. In event capturing, the event is first captured by the outermost element and propagated to the inner elements.

It's important to note that you can stop an event from bubbling further up the chain using the `event.stopPropagation()` method in the event handler. This can be useful in situations where you don't want parent elements responding to an event triggered on a child. But keep in mind that using `event.stopPropagation()` can sometimes lead to unexpected results, as it disrupts the normal flow of events.

# 4) Explain prototype

In JavaScript, every object has a special hidden property [[Prototype]] that’s either null or references another object. That object is called “a prototype”.

The prototype is essentially a "parent" object, and it is used as a fallback if a property is not found in the current object. If JavaScript fails to find a property in the object itself, it will look in the object's prototype, and then the prototype's prototype, and so on, up the chain until it either finds the property or hits the end of the chain (the root prototype, Object.prototype). This process is called "prototype chain" or "prototype inheritance".

For instance:

```javascript
let animal = {
  eats: true,
};

let rabbit = {
  jumps: true,
};

rabbit.__proto__ = animal; // sets animal as a prototype for rabbit

console.log(rabbit.eats); // true
console.log(rabbit.jumps); // true
```

Here, `rabbit` doesn't have the `eats` property, so JavaScript follows the [[Prototype]] reference and finds it in `animal`.

This is useful for sharing methods between objects, to reduce memory usage and improve lookup efficiency. Instead of each object having its own copy of each method, they can all share a single copy in the prototype:

```javascript
let animal = {
  eats: true,
  walk() {
    console.log("Animal walk");
  },
};

let rabbit = {
  jumps: true,
  __proto__: animal,
};

rabbit.walk(); // Animal walk
```

In this example, the `walk` method is found in the prototype, so it's shared between all objects that have `animal` as their prototype.

Finally, it's important to mention that while we can get and set the prototype of an object using `__proto__`, this property is considered outdated and somewhat deprecated, and the modern methods `Object.getPrototypeOf` and `Object.setPrototypeOf` or `Object.create` are preferred for getting and setting an object's prototype.

All JavaScript objects have a method called `hasOwnProperty` that can be used to check if a property is in the object itself or in its prototype chain. This method itself is inherited from `Object.prototype`.

# 5) What is a pure function

In JavaScript, a pure function is a function which:

1. **Always produces the same output for the same set of inputs**: This means that if you call a pure function with the same arguments multiple times, it will always return the same result.

2. **Has no side effects**: This means that the function doesn't change anything in the outside world, like modifying a global variable, changing the value of its arguments, making a HTTP request, writing to the console, or changing anything else outside its scope. It only provides the return value.

Here's an example of a pure function:

```javascript
function sum(a, b) {
  return a + b;
}
```

In this example, the `sum` function is pure because it always returns the same result if you call it with the same two numbers, and it doesn't have any side effects. It doesn't matter how many times or when you call it, if you call `sum(2, 3)`, it will always return `5`.

Pure functions are beneficial for several reasons:

1. **Testability**: Pure functions are easier to test because you don't need to mock anything, or prepare any setup or teardown; you just call the function with a set of inputs and check if the output is what you expect.

2. **Readability and Predictability**: When you're reading a pure function, you only need to keep track of the arguments. You don't have to worry about other parts of your program being changed unknowingly.

3. **Reusability**: Pure functions are more modular and can be reused across your application.

4. **Optimization**: Pure functions are great for performance optimizations. Since they always return the same result for the same input, you can use memoization to cache results for certain inputs.

5. **Functional Programming**: Pure functions are a core concept in functional programming, and are used for creating reliable, scalable and maintainable code.

# 6) How do you get the type of an object

In JavaScript, you can obtain the type of an object or a primitive value using the `typeof` operator. The `typeof` operator returns a string indicating the type of the unevaluated operand. Here's an example:

```javascript
console.log(typeof "Hello"); // "string"
console.log(typeof 5); // "number"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof { name: "Alice" }); // "object"
console.log(typeof [1, 2, 3]); // "object"
console.log(typeof null); // "object"
console.log(typeof function () {}); // "function"
```

However, as you can see in the examples above, `typeof` has limitations:

- For array or null, `typeof` will return "object". So if you want to differentiate between an object and an array, `typeof` will not be sufficient.
- If you want to check if a variable is null, `typeof` will not help either, because it incorrectly returns "object" for null values due to a bug in the language that has been kept for backward compatibility.

In such cases, you can use the `instanceof` operator, the `Array.isArray()` method, or the `constructor` property to determine the type:

```javascript
console.log([] instanceof Array); // true
console.log({} instanceof Object); // true
console.log("Hello" instanceof String); // false, because 'Hello' is not a String object, it's a primitive string value

console.log(Array.isArray([])); // true

console.log({}.constructor === Object); // true
console.log([].constructor === Array); // true
```

Remember, `instanceof` and `constructor` checks won't work across different realms like iframes or when using different contexts in Node.js. In those cases, you would need more complex checks, or avoid such situations.

# 7) When do you use: var, let or const?

The choice of `var`, `let`, or `const` in JavaScript depends on the scope and reassignment characteristics that you need for a variable.

**1. var:**

`var` is the oldest way to declare variables in JavaScript. Variables declared with `var` are function-scoped, meaning that their visibility is limited to the current function. If declared outside a function, `var` variables are globally scoped. They can be redeclared and updated.

```javascript
var x = 10; // x is 10

if (true) {
  var x = 20; // x is 20
}

console.log(x); // 20
```

In this case, `var` doesn't create a new scope within the `if` block. This can lead to unexpected behavior.

**2. let:**

`let` is a part of ES6 (ES2015) and is block-scoped, meaning that its visibility is limited to the current block of code (for example, if, for, while blocks or functions). Variables declared with `let` can't be redeclared in the same scope, but they can be updated.

```javascript
let x = 10; // x is 10

if (true) {
  let x = 20; // x is 20
}

console.log(x); // 10
```

In this case, `let` creates a new scope within the `if` block.

**3. const:**

`const` is also a part of ES6 and is block-scoped like `let`. However, `const` variables can neither be updated nor redeclared. This does not mean the value it holds is immutable, just that the variable identifier can't be reassigned. For instance, if the value is an object or an array, the elements in the object or array can be updated.

```javascript
const x = 10; // x is 10
x = 20; // error: assignment to constant variable
```

But:

```javascript
const arr = [1, 2, 3];
arr.push(4); // This is possible
console.log(arr); // [1, 2, 3, 4]

arr = [1]; // error: assignment to constant variable
```

In general, it's good practice to:

- Use `const` by default.
- Use `let` if you need to reassign a variable.
- Avoid `var` to prevent unwanted side effects due to its function scope.

This way, you can write predictable code and reduce potential bugs.

# 8) How many execution threads can be ran or a usually ran in Javascript?

JavaScript is single-threaded, which means it has a single call stack and can process one operation at a time. JavaScript engines run on a single thread of execution and carry out one statement at a time in order.

But JavaScript has features like async callbacks, promises, and async/await that allow it to deal with operations that might take a long time to complete (like fetching data from a server) without blocking the rest of the code execution. These features take advantage of the Event Loop and a mechanism called the Web APIs (provided by the hosting environment like a web browser or Node.js), which can handle these potentially blocking operations off the main JavaScript thread.

While those operations are being processed in the background, JavaScript can continue executing other code. When the asynchronous operation completes, it gets pushed onto the task queue. The Event Loop constantly checks if the call stack is empty, and if so, it will take the first task from the queue and push it onto the stack, which resumes execution of that task where it left off.

So, while JavaScript's execution is single-threaded, it can still handle asynchronous operations by using the Event Loop and Web APIs, which makes it behave in a non-blocking, asynchronous manner.

To handle computationally expensive tasks without blocking the main thread, Web Workers or Worker Threads (in Node.js) can be used. These are ways to run JavaScript in background threads, but they have their own separate global scopes — they don't share the same scope or have direct access to the main thread's memory. Communication between threads is done using messages.

# 9) Can you explain closures and provide an example?

A closure in JavaScript is a function that has access to its own scope, the outer function's scope, and the global scope. It also has access to variables from the parent function even after the parent function has finished executing. This is a direct result of how JavaScript's function scoping and function-level scope work.

Here's a simple example of a closure:

```javascript
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log("outerVariable:", outerVariable);
    console.log("innerVariable:", innerVariable);
  };
}

const newFunction = outerFunction("outside");
newFunction("inside"); // logs: outerVariable: outside, innerVariable: inside
```

In this case, `innerFunction` is a closure. It's a function defined inside `outerFunction` and it has access to `outerFunction`'s scope, which includes `outerVariable`.

When we call `outerFunction('outside')`, it returns `innerFunction`, and we store that returned function in `newFunction`. Even though `outerFunction` has finished executing and its scope would normally be gone, `newFunction` still has access to `outerVariable`, because `innerFunction` is a closure and it "closes over" its outer scope, keeping that scope alive.

This closure concept is very powerful in JavaScript and it's used in many patterns, like data privacy (through the Module Pattern), factory functions, and in JavaScript libraries and frameworks.

# 10) Explain the Module pattern

The Module pattern in JavaScript is a design pattern that provides a way to wrap a set of variables and functions into a single scoped object, similar to how classes, packages and modules work in other programming languages.

It leverages JavaScript's function scoping and closures to create private and public methods and properties. This can help organize code, provide encapsulation, and manage namespaces.

Here's a simple example:

```javascript
let MyModule = (function () {
  let privateVar = "I am private...";

  function privateMethod() {
    return `Private test: ${privateVar}`;
  }

  return {
    publicVar: "I am public!",
    publicMethod: function () {
      return (
        "I can see the private var from a public method: " + privateMethod()
      );
    },
  };
})();

console.log(MyModule.publicVar); // I am public!
console.log(MyModule.publicMethod());
// I can see the private var from a public method: Private test: I am private...
console.log(MyModule.privateVar); // undefined
console.log(MyModule.privateMethod()); // Uncaught TypeError: MyModule.privateMethod is not a function
```

In this example, `MyModule` is an Immediately Invoked Function Expression (IIFE) that returns an object.

Within the IIFE's scope, variables and functions are private — they can't be accessed directly from outside the IIFE. But the returned object has references to some of those private variables and functions, which makes them publicly accessible.

This allows us to create data and methods that are private (like `privateVar` and `privateMethod`), as well as data and methods that are public (like `publicVar` and `publicMethod`).

The Module pattern is a fundamental pattern in JavaScript and is used extensively in many modern JavaScript libraries and frameworks.

# 11) Can you explain how `this` keyword works in JavaScript?

The `this` keyword in JavaScript refers to the object that is currently calling the function where `this` is used. It's a special identifier that gets bound during the execution of functions.

However, the value of `this` is not determined by how or where a function is declared, but how it is called (the execution context). Here are some general rules:

1. **In the global scope:** `this` refers to the global object (usually `window` in a browser context, or `global` in Node.js).

```javascript
console.log(this); // window
```

2. **Within a function:** `this` depends on how the function is called. In a regular function call, `this` is the global object. However, in strict mode, `this` is `undefined`.

```javascript
function regularFunc() {
  console.log(this); // window (or undefined in strict mode)
}
regularFunc();
```

3. **Within a method:** `this` refers to the object that the method belongs to.

```javascript
const myObject = {
  prop: "I am an object property!",
  method: function () {
    console.log(this.prop);
  },
};
myObject.method(); // 'I am an object property!'
```

4. **Within an event handler:** `this` refers to the element that received the event.

```javascript
button.addEventListener("click", function () {
  console.log(this); // button element
});
```

5. **When a constructor function is used:** `this` refers to the newly created object.

```javascript
function MyConstructor() {
  this.prop = "I am a property!";
}
const myInstance = new MyConstructor();
console.log(myInstance.prop); // 'I am a property!'
```

6. **When using `call()`, `apply()`, or `bind()`:** `this` is explicitly set to the first argument of these functions.

```javascript
function logThis() {
  console.log(this);
}
const myObject = { prop: "I am an object!" };
logThis.call(myObject); // { prop: 'I am an object!' }
```

Remember, the value of `this` is determined by the calling context — it's not set when the function is declared, but when it's invoked. Understanding `this` can be a bit tricky because its behavior in JavaScript is somewhat different from how it behaves in many other languages. However, understanding `this` is crucial for writing robust and effective JavaScript code.

# 12) What's the difference between `==` and `===`?

In JavaScript, `==` and `===` are both comparison operators, but they behave differently when it comes to type coercion.

1. `==` (Double Equals) - Loose Equality:

   `==` compares two values for equality, and performs type coercion if necessary. In other words, it converts the operands to the same type before making the comparison. If the operands are of different types, it will try to convert one (or both) so that they are of the same type. For example:

   ```javascript
   console.log(1 == "1"); // true, because '1' is converted to a number
   console.log(true == 1); // true, because true is converted to a number (1)
   console.log(null == undefined); // true, because null and undefined are loosely equal
   ```

2. `===` (Triple Equals) - Strict Equality:

   `===` compares two values for equality, but does not perform type coercion. It will only return true if both the operands are of the same type and the contents match. For example:

   ```javascript
   console.log(1 === "1"); // false, because one is a number and the other is a string
   console.log(true === 1); // false, because one is a boolean and the other is a number
   console.log(null === undefined); // false, because one is null and the other is undefined
   ```

As a general rule, it's usually better to use `===` (and its counterpart `!==` for strict inequality) in your JavaScript code. This way, you can avoid some potentially confusing bugs and edge cases caused by type coercion. However, there can be valid use cases for `==` as well, such as when you intentionally want to exploit JavaScript's type coercion rules.

# 13) Can you explain the difference between `null` and `undefined`?

`null` and `undefined` in JavaScript both represent the absence of value. However, they are used in slightly different scenarios:

1. **undefined**: A variable is `undefined` if it has been declared, but no value has been assigned to it.

   ```javascript
   let test;
   console.log(test); // undefined
   ```

   It's also the value of function parameters for which no arguments are provided, and the return value of a function that doesn't explicitly return anything.

   ```javascript
   function demoFunction(param) {
     console.log(param);
   }

   demoFunction(); // undefined
   ```

   When an object property or array element doesn't exist, accessing it returns `undefined`.

   ```javascript
   let obj = {};
   console.log(obj.property); // undefined

   let arr = [];
   console.log(arr[1]); // undefined
   ```

2. **null**: `null` represents the intentional absence of any object value. It's often used when you want to explicitly assign no value or no object to a variable.

   ```javascript
   let test = null;
   console.log(test); // null
   ```

   Unlike `undefined`, `null` is not assigned by JavaScript itself; it's a value that programmers set when they want to indicate that a variable should be without value, typically to indicate the end of a value chain or that an object is intentionally absent.

In terms of comparison:

- `undefined == null` is `true`. This is because `==` performs type coercion and treats `undefined` and `null` as equivalent.
- `undefined === null` is `false`. This is because `===` (strict equality) doesn't perform type coercion, and `undefined` and `null` are different types.

Most of the time, when checking for both `null` and `undefined`, you can use `== null` or `!= null` checks in your code because of this coercion. But as a best practice, always try to use strict equality checks `===` and `!==` to avoid unexpected results due to coercion.

# 14) What is a Promise? How would you handle multiple asynchronous operations with Promises?

A `Promise` in JavaScript represents a proxy for a value not necessarily known when the promise is created. It allows you to handle asynchronous operations in a more flexible and readable manner. A `Promise` is in one of three states:

- `pending`: The Promise's outcome hasn't yet been determined, because the asynchronous operation that will produce its result hasn't completed yet.
- `fulfilled`: The asynchronous operation has completed, and the Promise has a resulting value.
- `rejected`: The asynchronous operation failed, and the Promise will never be fulfilled.

A `Promise` is said to be settled if it is either fulfilled or rejected.

A `Promise` can be used as follows:

```javascript
let promise = new Promise((resolve, reject) => {
    // asynchronous operation
    if (/* everything went well */) {
        resolve("Stuff worked!");
    }
    else {
        reject(Error("It broke"));
    }
});

promise.then(result => console.log(result), err => console.log(err));
```

To handle multiple asynchronous operations with promises, you can use `Promise.all` or `Promise.allSettled`:

1. `Promise.all` takes an array of promises and returns a new promise that only fulfills when all the promises in the array are fulfilled, or rejects as soon as one of them rejects.

```javascript
let promise1 = Promise.resolve(3);
let promise2 = 42;
let promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "foo");
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values); // expected output: Array [3, 42, "foo"]
});
```

If any promise is rejected, `Promise.all` immediately rejects with the reason of the first promise that rejected, discarding all the other promises whether or not they were resolved.

2. `Promise.allSettled` on the other hand waits until all promises have settled (each becomes either fulfilled or rejected). If all of the passed-in promises fulfill, or if any of the promises reject, `allSettled` returns a promise. This is useful when you do not care about the results of the promises, just whether or not they've completed.

```javascript
let promise1 = Promise.resolve(3);
let promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, "foo"));

Promise.allSettled([promise1, promise2]).then((results) =>
  results.forEach((result) => console.log(result.status))
);

// expected output (in any order):
// "fulfilled"
// "rejected"
```

Promises help to avoid callback hell and can be easier to read and maintain when dealing with complex asynchronous logic. Since ES7, async/await further simplifies working with Promises.

# 15) Can you explain how async/await works? And what is the difference with Promise?

`async/await` is a special syntax in JavaScript that's built on top of Promises, designed to make asynchronous code look and behave more like synchronous code. It's considered syntactic sugar over Promises, which means it provides a more readable and easier way to work with Promises, but doesn't introduce new functionality.

Here's how `async/await` works:

1. **async**: You put this keyword before a function to define it as an async function. An async function always returns a Promise. If the function has a return statement, the Promise will be resolved with that value. If the function throws an error, the Promise is rejected with that error.

```javascript
async function asyncFunc() {
  return "Hello, World!";
}
asyncFunc().then(alert); // alerts 'Hello, World!'
```

2. **await**: You can only use the `await` keyword inside an async function. `await` makes JavaScript pause and wait until a Promise resolves or rejects, and then resumes the execution of the async function and returns the resolved value.

```javascript
async function asyncFunc() {
  let response = await fetch("http://example.com"); // pause until fetch returns
  let data = await response.json(); // pause until the .json() method returns
  return data;
}
```

The key difference between `async/await` and Promises is the syntax and readability:

- With Promises, you often use `.then()` for handling successful operations and `.catch()` for handling errors. This can lead to callback hell when there's a lot of nested asynchronous operations.

- With `async/await`, the code appears as if it's synchronous, but it's actually asynchronous. Error handling can be done with regular `try/catch` blocks. This makes the code cleaner and easier to understand.

Note that `async/await` doesn't replace the need for Promises — it's built on top of them. It's simply a different way to handle asynchronous operations that some developers find more readable and intuitive.

# 16) What are callbacks and what issues might arise when using them?

A callback function in JavaScript is a function that is passed as an argument to another function, and is intended to be executed at a later time. The function that receives the callback function is often a higher order function.

Callbacks are often used when dealing with operations that are asynchronous, or take an indeterminate amount of time, such as requests to a server, file I/O operations, or timers.

Here's an example of a callback function:

```javascript
function greeting(name) {
  alert("Hello " + name);
}

function processUserInput(callback) {
  let name = prompt("Please enter your name.");
  callback(name);
}

processUserInput(greeting);
```

In this example, the `greeting` function is the callback function. It gets passed as an argument to `processUserInput`, and is called within that function.

Although callbacks are a fundamental part of JavaScript, they can lead to some issues:

1. **Callback Hell (or Pyramid of Doom)**: This is when multiple callbacks are nested within each other, leading to code that is hard to read and understand. This often happens when you try to execute multiple asynchronous operations that depend on the result of each other.

```javascript
getData(function (a) {
  getMoreData(a, function (b) {
    getEvenMoreData(b, function (c) {
      // do something with a, b and c
    });
  });
});
```

2. **Inversion of Control**: When we pass a callback to another function, we are trusting that function to call our callback at the right time, with the right arguments, and in the right context. This isn't always guaranteed, as the other function might contain a bug, or might not work as we expected.

3. **Error Handling**: Traditional error handling using try/catch does not work for asynchronous operations. Therefore, you often have to provide two callbacks: one for success, and one for error handling.

To mitigate callback hell, developers often use promises or async/await, which provide a cleaner syntax for chaining asynchronous operations. To deal with inversion of control and error handling, developers often use libraries or patterns that enforce certain constraints or conventions.

# 17) What are higher-order functions?

In JavaScript, functions are first-class objects. This means they can be assigned to variables, stored in data structures, passed as arguments to other functions, and returned from other functions. Because of this capability, JavaScript allows the creation and use of higher-order functions.

A higher-order function is a function that does at least one of the following:

1. Takes one or more functions as arguments.
2. Returns a function as its result.

Here are some examples of higher-order functions:

1. **Functions that accept other functions as arguments:**

The `Array` methods `map`, `filter`, and `reduce` are good examples of this. They each take a function as an argument:

```javascript
let numbers = [1, 2, 3, 4, 5];

let doubled = numbers.map(function (n) {
  return n * 2;
}); // [2, 4, 6, 8, 10]
```

In this example, `map` is a higher-order function that applies the provided function to each element in the array, returning a new array with the results.

2. **Functions that return other functions:**

```javascript
function greaterThan(n) {
  return function (m) {
    return m > n;
  };
}

let greaterThan10 = greaterThan(10);
console.log(greaterThan10(11)); // true
```

In this example, `greaterThan` is a higher-order function that produces a function for comparing values to the provided argument.

Higher-order functions are a key part of functional programming in JavaScript, and many JavaScript design patterns make use of them. They can make your code more abstract, versatile, and reusable.

# 18) What are classes in JavaScript and how do they work?

JavaScript classes, introduced in ECMAScript 2015 (ES6), are a syntactic sugar over JavaScript's existing prototype-based inheritance. This means that JavaScript classes provide a new syntax for creating objects and dealing with inheritance, but they don't fundamentally change how JavaScript's object-oriented programming works.

A JavaScript class can be defined as follows:

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  area() {
    return this.height * this.width;
  }
}
```

In this example, `Rectangle` is a class with a constructor and a single method, `area`. The `constructor` method is a special method for creating and initializing objects created with a class. You can create an instance of a class with the `new` keyword:

```javascript
let rect = new Rectangle(10, 20);
console.log(rect.area()); // 200
```

You can also use `extends` to create a subclass, and `super` to call the constructor of the superclass:

```javascript
class Square extends Rectangle {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }
}

let sq = new Square(5);
console.log(sq.area()); // 25
```

In this example, `Square` is a subclass of `Rectangle`. It uses `extends` to indicate inheritance, and `super` to call the `Rectangle` constructor.

Underneath the syntax, JavaScript classes are function objects. Therefore, typeof Rectangle in the example above would return "function".

Remember that JavaScript is prototype-based, and classes are a syntactic sugar over prototypes. The methods of a class are stored in the prototype property of the class, and they're shared among all instances of that class. This differs from the methods defined in the constructor or the instance itself, which would create a separate copy for each instance.

# 19) How would you improve the performance of a JavaScript web application?

Improving the performance of a JavaScript web application can involve a wide variety of techniques, depending on the specifics of the application. Here are some general strategies that can often help:

1. **Minification and Compression**: Minifying your JavaScript files removes unnecessary characters (like spaces and comments) without changing functionality. You can also enable Gzip compression on your server to reduce the size of files sent to the client.

2. **Bundling and Tree Shaking**: Using a module bundler like Webpack can help reduce the number of HTTP requests by combining your JavaScript files into one bundle. Tree shaking, a feature of some module bundlers, can eliminate unused code from the bundle, making it smaller.

3. **Lazy Loading**: Instead of loading the entire application when a user first visits, you can load only what's necessary for the initial view, and then lazily load the rest as needed.

4. **Caching**: Use service workers and the Cache API to cache your application's assets on the user's device, improving load times for return visits.

5. **Optimize Images**: Use appropriate file formats, and consider compressing images. You could also consider using responsive images, which adapt depending on the user's device.

6. **Debouncing and Throttling**: If you have event handlers on scrolling or resizing, or other rapidly firing events, debouncing or throttling those handlers can improve performance by limiting how often they can fire.

7. **Use the Production Build**: For libraries like React, there is a big difference between the development and production builds. Make sure you're using the production build for production.

8. **Avoid Memory Leaks**: Unused objects that haven't been properly deallocated can consume memory and cause the app to run slower. Be sure to clear intervals and timeouts, and remove event listeners when they're no longer needed.

9. **Optimize Render Performance**: Avoid unnecessary re-renders of components (for instance, in React, consider shouldComponentUpdate, React.memo, or PureComponent). Also, take advantage of virtualized lists for rendering large lists of data.

10. **Use Web Workers for Heavy Computations**: Web Workers allow you to run JavaScript in background threads, freeing up the main thread for UI updates.

11. **Optimize Database Operations**: Ensure your queries are performant, use indexing where necessary, and consider caching frequently accessed data.

Remember, the first step to optimizing is to find out what needs to be optimized. Use performance profiling tools in the browser to identify bottlenecks. Only optimize the parts of your application that are actually causing performance issues - premature optimization can lead to harder-to-read code and wasted time.

# 20) Can you discuss ways to profile and debug JavaScript code?

Profiling and debugging JavaScript code is a critical part of the development process. Here are several ways you can approach this:

1. **Chrome DevTools**: The Chrome browser's built-in developer tools provide a variety of capabilities for profiling and debugging JavaScript code.

   - **Sources Panel**: Allows you to view files, set breakpoints, and step through your code one line at a time.
   - **Console**: Displays logged messages and JavaScript errors, and allows you to run JavaScript expressions on the fly.
   - **Network Panel**: Lets you see how your files are loading and how long each one takes, which can be crucial for performance profiling.
   - **Performance Panel**: Allows you to record and analyze the activity of your website as it runs, providing detailed insight into where time is spent.
   - **Memory Panel**: Allows you to take heap snapshots and analyze memory usage, which can help identify memory leaks.

2. **Firefox Developer Tools**: Similar to Chrome, Firefox's built-in developer tools also provide a wide range of features for debugging and profiling JavaScript.

3. **Safari Web Inspector**: Safari's Web Inspector provides similar functionality to the aforementioned tools, including resources, timelines, and debugging panels.

4. **Node.js Debugging**: If you're working with Node.js, you can use built-in debugging tools such as Node Inspector, or the `--inspect` and `--inspect-brk` flags when starting your Node.js script.

5. **Visual Studio Code**: This popular code editor provides an excellent built-in debugger for JavaScript and Node.js. You can set breakpoints, step through code, inspect variables, and more.

6. **Logging**: Using `console.log()`, `console.error()`, `console.warn()`, etc. for debugging is a simple and widely used approach. In addition, `console.trace()` can be used to output a stack trace to the console, which can be useful for tracing function calls.

7. **Error Tracking Services**: Error tracking services such as Sentry or Rollbar can capture errors in real-time from your production environment, providing stack traces, context, and analytics about JavaScript errors.

8. **Unit Testing**: By writing unit tests for your code (e.g., with frameworks such as Jest or Mocha), you can catch bugs and prevent regressions. These tests can be used to ensure that individual units of code (like functions or methods) are behaving as expected.

9. **Linting**: Tools like ESLint can analyze your code for potential errors, enforce coding standards, and even automatically fix some types of problems.

10. **Profiling Tools**: Tools like `node --prof` and `node --prof-process` can be used to generate and analyze CPU profiles, helping you understand which parts of your JavaScript code are consuming the most CPU time.

When debugging, remember the basic process: reproduce the error consistently, understand what should happen vs. what is actually happening, and then systematically narrow down the place where the bug occurs until you find the root cause. Profiling, on the other hand, is about identifying bottlenecks and understanding the overall performance characteristics of your application.

# 21) What new features were introduced in ES6 (ES2015) and later versions that you find particularly useful?

The ECMAScript 6 (ES6) standard, also known as ES2015, and later versions introduced a significant number of important new features and improvements. Some of the most notable and useful ones include:

1. **`let` and `const` keywords**: These keywords introduced block scoping to JavaScript. `let` allows you to declare variables limited to the scope of a block statement, while `const` is used to declare variables whose values are never intended to change. These constructs help avoid certain types of bugs and improve code readability.

2. **Arrow functions**: Arrow functions provide a more concise syntax for writing function expressions. They also lexically bind the `this` value, which can be very useful.

3. **Template literals**: Template literals (template strings) allow embedded expressions and improve the process of concatenating strings, enabling a cleaner, more readable code.

4. **Promises**: Promises provide a more powerful and flexible way of dealing with asynchronous operations, reducing callback hell.

5. **Modules (import/export)**: ES6 introduced a native module system, which allows for the static importing and exporting of variables, functions, classes, etc.

6. **Classes**: Although JavaScript remains prototype-based, ES6 classes provide a much cleaner and more intuitive syntax for creating objects and dealing with inheritance.

7. **Destructuring assignment**: Destructuring assignment syntax allows you to unpack values from arrays or properties from objects, into distinct variables.

8. **Spread and Rest operators (`...`)**: These operators allow for more efficient manipulation of arrays and objects.

9. **Default parameters**: ES6 allows function parameters to be set by default, in case no or undefined values are passed into the function.

10. **Async/Await**: Introduced in ES2017, async/await is a special syntax that makes working with promises more comfortable and easier to read.

11. **Optional chaining (`?.`) and Nullish coalescing (`??`)**: Introduced in ES2020, these operators allow you to write more concise and safe code when dealing with chained properties and dealing with `null` or `undefined` values.

12. **BigInt**: Also introduced in ES2020, BigInt is a built-in object that provides a way to represent whole numbers larger than `2^53 - 1`, which is the upper limit for a standard JavaScript `Number`.

These features, among many others introduced in ES6 and later versions, greatly enhance JavaScript's expressiveness and power, and they're now a fundamental part of modern JavaScript development.

# 22) Can you explain destructuring and how you would use it?

Destructuring in JavaScript is a feature introduced in ES6 (ES2015) that provides a way to unpack values from arrays, or properties from objects, into distinct variables. This can make your code more readable and less verbose.

**Array Destructuring:**

Consider an array as follows:

```javascript
let array = ["apple", "banana", "cherry"];
```

You can use array destructuring to assign the values of the array to variables:

```javascript
let [fruit1, fruit2, fruit3] = array;
console.log(fruit1); // 'apple'
console.log(fruit2); // 'banana'
console.log(fruit3); // 'cherry'
```

You can also skip over elements if you only want specific ones:

```javascript
let [, , fruit3] = array;
console.log(fruit3); // 'cherry'
```

**Object Destructuring:**

Similarly, you can destructure objects. Consider an object as follows:

```javascript
let obj = { name: "John", age: 30, city: "New York" };
```

You can use object destructuring to assign the properties of the object to variables:

```javascript
let { name, age, city } = obj;
console.log(name); // 'John'
console.log(age); // 30
console.log(city); // 'New York'
```

You can also assign to variables with a different name:

```javascript
let { name: firstName, age: years } = obj;
console.log(firstName); // 'John'
console.log(years); // 30
```

**Destructuring Function Parameters:**

Destructuring can also be useful when dealing with function parameters. For instance, if a function accepts an object as a parameter, you can directly destructure the object in the function definition:

```javascript
function greet({ name, age }) {
  console.log(`Hello, my name is ${name} and I'm ${age} years old.`);
}

greet(obj); // "Hello, my name is John and I'm 30 years old."
```

Overall, destructuring can make your code more clear and concise, and it's particularly useful when working with complex data structures or when you only need certain parts of an array or object.

# ES6
