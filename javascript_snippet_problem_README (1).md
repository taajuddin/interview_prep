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

## 🌀 9. Advanced Closures & Currying
```js
function add(a) {
  return function(b) {
    return a + b;
  };
}
console.log(add(5)(3));
```
**Output:** `8`
**Explanation:** Function currying — inner function remembers a.

```js
function multiply(a) {
  return function(b, c) {
    return a * b * c;
  };
}
console.log(multiply(2)(3, 4));
```
**Output:** `24`

## 🔗 10. Advanced this and Binding
```js
const person = {
  name: "Alice",
  sayHi() {
    return this.name;
  }
};
const hi = person.sayHi;
console.log(hi());
```
**Output:** `undefined`
```js
const person = {
  name: "Alice",
  sayHi() {
    return this.name;
  }
};
const hi = person.sayHi.bind(person);
console.log(hi());
```
**Output:** `Alice`
```js
function show() {
  console.log(this);
}
show.call(5);
```
**Output:** `Number {5}`
**Explanation:** Primitive 5 is wrapped as an object.
```js
const user = {
  name: "Bob",
  arrow: () => console.log(this.name),
  normal() { console.log(this.name); }
};
user.arrow();
user.normal();
-----
Output:
Copy code
undefined
Bob
```
```js
function A() {
  this.x = 10;
  return 20;
}
console.log(new A().x);
```
---
**Output:** 10
**Explanation:** Returned primitive ignored in constructor.
```
```js
const f = {
  name: "JS",
  print: function() {
    setTimeout(function() {
      console.log(this.name);
    }, 0);
  }
};
f.print();
Output: undefined
```
```js
const f = {
  name: "JS",
  print: function() {
    setTimeout(() => console.log(this.name), 0);
  }
};
f.print();
Output: JS
Explanation: Arrow inherits this from f.
```
## 11. Deep Type Coercion
```js
58️⃣
console.log(true + false);

Output: 1

59️⃣
console.log("5" - 2);

Output: 3

60️⃣
console.log([] + false - null + true);

Output: NaN

61️⃣
console.log('10' - '4' - '3' - 2 + '5');

Output: "15"

62️⃣
console.log(+'10' + +'10');

Output: 20

63️⃣
console.log([] == 0);
console.log('' == 0);
console.log('0' == 0);

Output:
true
true
true


64️⃣
console.log(false == []);
console.log(false == {});
console.log(false == null);

Output:
true
false
false
```

## ⚙️ 12. Destructuring, Spread & Rest
```js
65️⃣
const {a, b} = {a: 1, b: 2};
console.log(a, b);

Output: 1 2

66️⃣
const [x, , y] = [1, 2, 3];
console.log(x, y);

Output: 1 3

67️⃣
const {a = 5, b = 7} = {a: 1};
console.log(a, b);

Output: 1 7

68️⃣
const arr = [1, 2, 3];
const arr2 = [...arr];
arr2.push(4);
console.log(arr, arr2);

Output: [1,2,3] [1,2,3,4]

69️⃣
const obj = {x: 1, y: 2};
const clone = {...obj, z: 3};
console.log(clone);

Output: {x:1,y:2,z:3}

70️⃣
function sum(...args) {
  return args.reduce((a, b) => a + b);
}
console.log(sum(1, 2, 3));

Output: 6

71️⃣
const [a, ...rest] = [1, 2, 3, 4];
console.log(a, rest);

Output: 1 [2,3,4]
```
## 🧬 13. Prototype & Class Inheritance
```
72️⃣
function Animal(name) {
  this.name = name;
}
Animal.prototype.sound = function() {
  return "Generic";
};
function Dog(name) {
  Animal.call(this, name);
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.sound = function() {
  return "Bark";
};
console.log(new Dog("Max").sound());

Output: Bark

73️⃣
class A {
  say() { return 'A'; }
}
class B extends A {
  say() { return super.say() + 'B'; }
}
console.log(new B().say());

Output: AB

74️⃣
class Counter {
  static count = 0;
  constructor() { Counter.count++; }
}
new Counter();
new Counter();
console.log(Counter.count);

Output: 2

75️⃣
class User {
  constructor(name) { this.name = name; }
  static greet() { return 'Hi'; }
}
const u = new User('JS');
console.log(User.greet());

Output: Hi

⏱️ 14. Async/Await Edge Cases

76️⃣
async function f() {
  return 5;
}
f().then(console.log);

Output: 5

77️⃣
async function f() {
  throw 10;
}
f().catch(console.log);

Output: 10

78️⃣
async function f() {
  await new Promise(r => setTimeout(r, 0));
  console.log('done');
}
console.log('start');
f();
console.log('end');

Output:
start
end
done


79️⃣
(async () => {
  console.log(await Promise.resolve('x'));
})();

Output: x

80️⃣
Promise.resolve(1)
  .then(x => x + 1)
  .then(x => { throw x; })
  .catch(x => x + 1)
  .then(x => console.log(x));

Output: 3
```
## 🔄 15. Event Loop & Microtasks
```js
81️⃣
console.log('1');
setTimeout(() => console.log('2'));
Promise.resolve().then(() => console.log('3'));
console.log('4');

Output:
1
4
3
2


82️⃣
Promise.resolve().then(() => console.log('micro'));
queueMicrotask(() => console.log('queue'));
setTimeout(() => console.log('macro'));

Output:
micro
queue
macro
```

## 🧮 16. Tricky Operators
```js
83️⃣
console.log(1 + - + + + -1);

Output: 0

84️⃣
console.log((true + false) > 2 + true);

Output: false

85️⃣
console.log([] + []);
console.log([] + {});
console.log({} + []);

Output:
""
"[object Object]"
"[object Object]"


86️⃣
console.log([] == '');
console.log([null] == '');

Output:
true
true
```

## 🧠 17. Object & Array Traps
```js
87️⃣
const arr = [1, 2, 3];
arr[10] = 99;
console.log(arr.length);

Output: 11

88️⃣
console.log([,,,].length);

Output: 3

89️⃣
const obj = {a: 1, b: 2};
console.log(Object.keys(obj).length);

Output: 2

90️⃣
console.log(Object.assign({}, {a: 1}, {a: 2, b: 3}));

Output: {a:2,b:3}

91️⃣
console.log(JSON.stringify({a: undefined, b: null}));

Output: {"b":null}

92️⃣
console.log([1,2,3].map(parseInt));

Output: [1, NaN, NaN]

93️⃣
console.log(!!"false" == !!"true");

Output: true
```
## 🎲 18. Miscellaneous Mind-Twisters
```js
94️⃣
console.log(typeof typeof 1);

Output: "string"

95️⃣
console.log((function(){ return typeof arguments; })());

Output: "object"

96️⃣
console.log(isNaN('NaN'));

Output: true

97️⃣
console.log(+true === 1);
console.log(+false === 0);

Output: true true

98️⃣
console.log(1 < 2 < 3);
console.log(3 > 2 > 1);

Output:
true
false


99️⃣
console.log([] == []);
console.log({} == {});

Output: false false

100️⃣
console.log(typeof NaN);

Output: "number"

101️⃣
console.log(0.1 + 0.2 === 0.3);

Output: false

102️⃣
console.log([1, 2] + [3, 4]);

Output: "1,23,4"

103️⃣
console.log('' == 0);

Output: true

104️⃣
console.log(!!null);
console.log(!!undefined);
console.log(!!NaN);

Output: false false false

105️⃣
console.log(1 && 2 && 3);

Output: 3

106️⃣
console.log(0 || 2 || 4);

Output: 2

107️⃣
console.log(false || 'Hello');

Output: 'Hello'

108️⃣
console.log('Hello' && false);

Output: false

109️⃣
console.log(!!'Hello');

Output: true

110️⃣
console.log(typeof (class {}));

Output: "function"

111️⃣
console.log(typeof Symbol());

Output: "symbol"

112️⃣
console.log(typeof BigInt(10));

Output: "bigint"

113️⃣
console.log(0 == false);
console.log(0 === false);

Output:
true
false


114️⃣
console.log(NaN === NaN);

Output: false

115️⃣
console.log(Object.is(NaN, NaN));

Output: true

116️⃣
console.log(Math.max() < Math.min());

Output: true

117️⃣
console.log([] + {});
console.log({} + []);

Output:
"[object Object]"
"[object Object]"


118️⃣
console.log('10' > '2');

Output: false
Explanation: Lexicographical comparison.

119️⃣
console.log(('b' + 'a' + + 'a' + 'a').toLowerCase());

Output: "banana"

120️⃣
console.log(1 == '1');
console.log(1 === '1');

Output:
true
false
```
---
