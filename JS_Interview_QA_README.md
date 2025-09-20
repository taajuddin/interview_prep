# JavaScript Interview Q&A — README

## Table of Contents
- JavaScript Fundamentals
- Core Concepts
- DOM & Browser
- Asynchronous JavaScript
- ES6+ Features
- Advanced Topics
- Code Examples
- Practice Questions
- Additional Resources

---

## 🎯 JavaScript Fundamentals

### Q: What is JavaScript?
JavaScript is a high-level, interpreted programming language used to make web pages interactive. It is single-threaded, dynamically typed, and follows the **ECMAScript** standard.

### Q: Difference between `var`, `let`, and `const`?
- `var`: function-scoped, hoisted, can be re-declared.
- `let`: block-scoped, not hoisted in the same way, cannot be re-declared in the same scope.
- `const`: block-scoped, must be initialized, cannot be reassigned.

```javascript
var x = 10;
let y = 20;
const z = 30;
```

### Q: What are primitive data types in JavaScript?
- String
- Number
- BigInt
- Boolean
- Undefined
- Null
- Symbol

---

## 🔧 Core Concepts

### Q: What is a closure?
A closure is a function that remembers its lexical scope even when executed outside of it.

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  }
}
const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

### Q: What is the difference between `==` and `===`?
- `==` → abstract equality (performs type coercion)
- `===` → strict equality (no type coercion)

```javascript
0 == '0'   // true
0 === '0'  // false
```

### Q: Explain prototypal inheritance.
Objects in JavaScript inherit directly from other objects via the **prototype chain**.

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

const john = new Person("John");
console.log(john.greet()); // Hello, John
```

---

## 🌐 DOM & Browser

### Q: What is the DOM?
The Document Object Model (DOM) is a programming interface for HTML and XML documents. It represents the page so programs can manipulate structure, style, and content.

### Q: Difference between `document.getElementById` and `querySelector`?
- `getElementById`: Selects element by ID, returns a single element.
- `querySelector`: Selects first matching element using CSS selectors.

```javascript
document.getElementById("myDiv");
document.querySelector(".myClass");
```

### Q: What is event bubbling and capturing?
- **Bubbling**: Event propagates from child to parent.
- **Capturing**: Event propagates from parent to child.

```javascript
element.addEventListener("click", handler, true);  // Capturing
element.addEventListener("click", handler, false); // Bubbling
```

---

## ⚡ Asynchronous JavaScript

### Q: How does the event loop work?
JavaScript uses an **event loop** to handle async operations. The call stack executes code, async tasks are sent to the Web APIs, and callbacks go into the task queue, then back to the stack.

### Q: Difference between callback, promise, and async/await?
- **Callback**: Function passed into another function.
- **Promise**: Represents a value that will be resolved in the future.
- **Async/Await**: Syntactic sugar for promises, makes async code look synchronous.

```javascript
// Promise
fetch("https://api.example.com")
  .then(res => res.json())
  .then(data => console.log(data));

// Async/Await
async function fetchData() {
  const res = await fetch("https://api.example.com");
  const data = await res.json();
  console.log(data);
}
```

---

## 🚀 ES6+ Features

### Q: What are some new features in ES6?
- Arrow functions
- Template literals
- Destructuring
- Spread/rest operators
- Classes
- Modules
- Promises
- Default parameters

```javascript
const add = (a, b = 5) => a + b;
console.log(add(10)); // 15
```

### Q: What is destructuring?
Extracting values from arrays/objects into variables.

```javascript
const user = { name: "Alice", age: 25 };
const { name, age } = user;
console.log(name, age);
```

---

## 👨‍💼 Advanced Topics

### Q: Explain `this` keyword.
- In a regular function, `this` refers to the global object (`window` in browsers).
- In a method, `this` refers to the object.
- In arrow functions, `this` is lexically scoped.

### Q: What is a higher-order function?
A function that takes another function as an argument or returns one.

```javascript
function multiplyBy(factor) {
  return function(num) {
    return num * factor;
  };
}
const double = multiplyBy(2);
console.log(double(5)); // 10
```

### Q: What is debouncing and throttling?
- **Debouncing**: Group multiple events and trigger only once after a delay.
- **Throttling**: Ensure function runs at most once per interval.

---

## 💻 Code Examples

### Example: Debounce function
```javascript
function debounce(fn, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}
```

### Example: Throttle function
```javascript
function throttle(fn, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```

---
### Q: Explain the difference between `null` and `undefined`.
- `undefined`: A variable has been declared but not assigned a value.  
- `null`: An assigned value that represents "no value" or "empty".  

```javascript
let a;
console.log(a); // undefined
let b = null;
console.log(b); // null
```

### Q: What are arrow functions and how are they different from regular functions?
- Shorter syntax for writing functions.  
- Do not have their own `this`, `arguments`, `super`, or `new.target`.  
- Cannot be used as constructors.  

```javascript
const add = (x, y) => x + y;
```

### Q: How does JavaScript handle type coercion?
JavaScript automatically converts values from one type to another when required.  
- Implicit coercion: `"5" * 2 → 10`  
- Explicit coercion: `Number("5") → 5`  

### Q: Explain `call`, `apply`, and `bind`.
- `call`: Invokes a function with a given `this` and arguments passed individually.  
- `apply`: Similar to `call`, but arguments are passed as an array.  
- `bind`: Returns a new function with `this` permanently bound.  

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
const person = { name: "Alice" };
greet.call(person, "Hello");
greet.apply(person, ["Hi"]);
const bound = greet.bind(person);
bound("Hey");
```

### Q: How does `async/await` improve readability over promises?
- Makes async code look synchronous.  
- Easier to read and debug than chained `.then()` calls.  

```javascript
async function fetchData() {
  try {
    const res = await fetch("/api");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

### Q: What is the difference between `map`, `filter`, and `reduce`?
- `map`: Transforms each element → returns a new array.  
- `filter`: Selects elements based on condition → returns a new array.  
- `reduce`: Accumulates values → returns a single value.  

```javascript
const arr = [1, 2, 3, 4];
console.log(arr.map(x => x * 2));     // [2, 4, 6, 8]
console.log(arr.filter(x => x % 2)); // [1, 3]
console.log(arr.reduce((a, b) => a + b, 0)); // 10
```

### Q: What is the difference between shallow copy and deep copy in JS?
- **Shallow copy**: Copies only the first level; nested objects share references.  
- **Deep copy**: Creates entirely new nested objects.  

```javascript
const obj = { a: 1, b: { c: 2 } };
const shallow = { ...obj };
shallow.b.c = 99; 
console.log(obj.b.c); // 99 (affected)

const deep = JSON.parse(JSON.stringify(obj));
deep.b.c = 42;
console.log(obj.b.c); // 99 (not affected)
```

### Q: How do you handle errors in async/await?
Use `try...catch` blocks to handle errors gracefully.  

```javascript
async function getUser() {
  try {
    const res = await fetch("/user");
    return await res.json();
  } catch (err) {
    console.error("Error fetching user:", err);
  }
}
```
