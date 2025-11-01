# JavaScript Snippet Problems (Core Concepts)

This repository contains 115+ JavaScript snippet interview problems covering closures, scope, hoisting, async/await, promises, coercion, and the event loop — each with outputs and detailed explanations.

## Example

### 🧩 Problem 1
```js
for (var i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 0);
}
```
**Output:**
```
5
5
5
5
5
```

**Explanation:**
Because `var` is function-scoped, all callbacks share the same variable `i`. By the time the loop finishes, `i = 5`.

---

(Full file includes 115+ such problems grouped by topic.)
