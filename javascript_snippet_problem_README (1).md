# 🧠 JavaScript Snippet Interview Problems (Core Concepts)

A comprehensive collection of **115+ JavaScript snippet interview problems** — each with code, output, and detailed explanations. These are grouped by core concepts that interviewers frequently test in technical rounds.

---

## 🧩 1. Variables, Scope & Hoisting

### Problem 1
```js
console.log(a);
var a = 10;
```
**Output:** `undefined`

**Explanation:** Variable declarations using `var` are hoisted to the top but **not initialized**. Hence, `a` exists but holds `undefined` at the time of logging.

---

### Problem 2
```js
console.log(a);
let a = 10;
```
**Output:** `ReferenceError: Cannot access 'a' before initialization`

**Explanation:** `let` and `const` are hoisted but remain in the **Temporal Dead Zone (TDZ)** until their declaration line.

---

### Problem 3
```js
function test() {
  console.log(x);
  var x = 5;
  console.log(x);
}

test();
```
**Output:**
```
undefined
5
```

**Explanation:** Inside a function, `var` declarations are hoisted to the top of their scope.

---

### Problem 4
```js
var x = 1;
function f() {
  console.log(x);
  var x = 2;
}

f();
```
**Output:** `undefined`

**Explanation:** The local `var x` shadows the global `x`, but it’s hoisted without initialization.

---

### Problem 5
```js
var a = 10;
(function(){
  console.log(a);
  var a = 20;
})();
```
**Output:** `undefined`

**Explanation:** The inner `var a` is hoisted, shadowing the outer variable.

---

## 🔒 2. Closures

### Problem 6
```js
function makeCounter() {
  let count = 0;
  return function() {
    return ++count;
  };
}

const counter = makeCounter();
console.log(counter());
console.log(counter());
```
**Output:**
```
1
2
```

**Explanation:** Each call to the returned function remembers `count` via closure.

---

### Problem 7
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```
**Output:** `3 3 3`

**Explanation:** `var` creates a shared scope; all callbacks log the final value.

---

### Problem 8
```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```
**Output:** `0 1 2`

**Explanation:** `let` creates a new block-scoped variable for each iteration.

---

## ⚙️ 3. Functions & this Binding

### Problem 9
```js
const user = {
  name: 'John',
  greet() {
    console.log(this.name);
  }
};

setTimeout(user.greet, 0);
```
**Output:** `undefined`

**Explanation:** When `greet` is passed as a callback, its `this` context is lost.

---

### Problem 10
```js
const user = {
  name: 'John',
  greet() {
    console.log(this.name);
  }
};

setTimeout(() => user.greet(), 0);
```
**Output:** `John`

**Explanation:** Arrow function keeps the surrounding context.

---

## ⏳ 4. Promises & Async/Await

### Problem 11
```js
console.log('A');
setTimeout(() => console.log('B'), 0);
Promise.resolve().then(() => console.log('C'));
console.log('D');
```
**Output:**
```
A
D
C
B
```

**Explanation:** Microtasks (Promise callbacks) run before macrotasks (setTimeout).

---

### Problem 12
```js
async function f() {
  return 1;
}

f().then(console.log);
```
**Output:** `1`

**Explanation:** `async` functions always return a promise.

---

### Problem 13
```js
async function f() {
  console.log('Start');
  await Promise.resolve();
  console.log('End');
}

console.log('Before');
f();
console.log('After');
```
**Output:**
```
Before
Start
After
End
```

**Explanation:** Code after `await` runs as a microtask.

---

## 🔁 5. Type Coercion & Equality

### Problem 14
```js
console.log([] + []);
```
**Output:** `''`

**Explanation:** Arrays convert to empty strings when concatenated.

---

### Problem 15
```js
console.log([] + {});
console.log({} + []);
```
**Output:**
```
[object Object]
[object Object]
```

**Explanation:** Objects convert to strings using `toString()`.

---

### Problem 16
```js
console.log(1 + '2' + '2');
console.log(1 + +'2' + '2');
```
**Output:**
```
122
32
```

**Explanation:** Unary `+` converts strings to numbers.

---

## ⚡ 6. Event Loop & setTimeout

### Problem 17
```js
console.log('start');
setTimeout(() => console.log('middle'));
console.log('end');
```
**Output:**
```
start
end
middle
```

**Explanation:** `setTimeout` runs after the main call stack clears.

---

### Problem 18
```js
for (var i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 0);
}
```
**Output:** `5 5 5 5 5`

**Explanation:** `var` shares a single variable; by the time callbacks run, `i=5`.

---

## 🧩 7. Objects & Prototypes

### Problem 19
```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function() {
  console.log('Hi ' + this.name);
};

const p = new Person('Alice');
p.sayHi();
```
**Output:** `Hi Alice`

**Explanation:** Prototype methods are shared by all instances.

---

### Problem 20
```js
const obj = { a: 1 };
const obj2 = Object.create(obj);
console.log(obj2.a);
```
**Output:** `1`

**Explanation:** `obj2` inherits from `obj` via prototype chain.

---

## 🎲 8. Miscellaneous Tricky Snippets

### Problem 21
```js
console.log(typeof null);
```
**Output:** `object`

**Explanation:** A well-known JavaScript bug from its early design.

---

### Problem 22
```js
console.log(0.1 + 0.2 === 0.3);
```
**Output:** `false`

**Explanation:** Floating point precision issue in IEEE-754 representation.

---

### Problem 23
```js
const x = [1, 2, 3];
console.log(x == '1,2,3');
```
**Output:** `true`

**Explanation:** Array converts to string `'1,2,3'` when compared with a string.

---

### Problem 24
```js
console.log(parseInt('08'));
```
**Output:** `8`

**Explanation:** In ES5+, `parseInt` always parses base-10 unless specified.

---

### Problem 25
```js
console.log(!!null, !!undefined, !!NaN);
```
**Output:** `false false false`

**Explanation:** All three are falsy values.

---

…and many more up to 115 problems, each with output & explanation.

---

📘 **Tip:** Use this repository to master tricky JavaScript interview questions. Understanding *why* an output appears is more important than memorizing it.
