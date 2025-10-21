# React.js Interview Questions & Answers

This document contains frequently asked **React.js interview questions** with simple explanations and examples.

---

## 1. What is React?
- React is a **JavaScript library** developed by Facebook for building **user interfaces**.
- It uses a **component-based architecture**.
- Works with a **virtual DOM** for fast rendering.

---

## 2. What are the main features of React?
- Declarative UI
- Component-based
- Virtual DOM
- One-way data binding
- Strong community support

---

## 3. What is JSX?
- JSX stands for **JavaScript XML**.
- It allows writing HTML-like syntax inside JavaScript.

```jsx
const element = <h1>Hello, React!</h1>;
```

---

## 4. What are Functional and Class Components?
- **Functional Component**: Simple JS function that returns JSX.
```jsx
function Greeting() {
  return <h1>Hello World</h1>;
}
```

- **Class Component**: ES6 class with `render()` method.
```jsx
class Greeting extends React.Component {
  render() {
    return <h1>Hello World</h1>;
  }
}
```

---

## 5. What is Virtual DOM?
- A lightweight copy of the **real DOM**.
- React updates the Virtual DOM first, compares it with the previous version (**diffing**), and updates only the changed parts in the real DOM (**reconciliation**).

---

## 6. What are React Hooks?
- Functions that let you use state and lifecycle methods in functional components.
- Common hooks:
  - `useState` → for state
  - `useEffect` → for side effects
  - `useContext` → for context API
  - `useReducer` → for complex state logic

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

---

## 7. What is useEffect Hook used for?
- Used for performing **side effects** (API calls, event listeners, subscriptions).

```jsx
import { useState, useEffect } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []);

  return <ul>{users.map(user => <li key={user.id}>{user.name}</li>)}</ul>;
}
```

---

## 8. What is the difference between Props and State?
| Feature | Props | State |
|---------|-------|-------|
| Mutability | Immutable (read-only) | Mutable (can change) |
| Ownership | Passed from parent to child | Managed inside component |
| Usage | Communication between components | Store component data |

---

## 9. What are Controlled and Uncontrolled Components?
- **Controlled Component** → Form data is handled by React state.
```jsx
function ControlledInput() {
  const [value, setValue] = useState("");
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}
```

- **Uncontrolled Component** → Form data handled by the DOM.
```jsx
function UncontrolledInput() {
  const inputRef = useRef();
  return <input ref={inputRef} />;
}
```

---

## 10. What is React Router?
- A library for handling navigation in React apps.
- Provides components like `<BrowserRouter>`, `<Route>`, `<Link>`, `<Navigate>`.

```jsx
import { BrowserRouter, Route, Routes, Link } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<h1>Home</h1>} />
        <Route path="/about" element={<h1>About</h1>} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## 11. What is Context API?
- A way to share values (like theme, user info) without prop drilling.

```jsx
const ThemeContext = React.createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <h1>Theme is {theme}</h1>;
}
```

---

## 12. Difference between useMemo and useCallback?
- **useMemo** → Memoizes **values**.
- **useCallback** → Memoizes **functions**.

```jsx
const memoizedValue = useMemo(() => expensiveCalculation(a, b), [a, b]);
const memoizedFn = useCallback(() => handleClick(id), [id]);
```

---
## ⚛️ useMemo in React

### 🧠 Definition
`useMemo` is a React Hook that **memoizes the result of a calculation** — meaning it **caches the computed value** and only re-calculates it when one of its dependencies changes.

It is mainly used to **optimize expensive calculations** or **prevent unnecessary re-computations** on every render.

---
## ⚛️ useMemo in React

### 🧠 Definition
`useMemo` is a React Hook that **memoizes the result of a calculation** — meaning it **caches the computed value** and only re-calculates it when one of its dependencies changes.
It is mainly used to **optimize expensive calculations** or **prevent unnecessary re-computations** on every render.

---

### 🔹 Example 1: With `useMemo`

```jsx
import React, { useState, useMemo } from "react";

function ExpensiveCalculation() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // ✅ Memoize the expensive calculation
  const expensiveValue = useMemo(() => {
    console.log("Calculating...");
    let total = 0;
    for (let i = 0; i < 100000000; i++) {
      total += i;
    }
    return total + count;
  }, [count]); // Only re-run when `count` changes

  return (
    <div>
      <h2>Expensive Value: {expensiveValue}</h2>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />
    </div>
  );
}

export default ExpensiveCalculation;
```
## 🧩 Explanation:
- useMemo stores the result of the expensive calculation.
- When you type in the input, React re-renders, but the cached value is used.
- The calculation only re-runs when count changes.
# ⚡ Summary Table
| Scenario          | Computation Runs On Every Render? | Memoized Value? | Explanation                        |
| ----------------- | --------------------------------- | --------------- | ---------------------------------- |
| Without `useMemo` | ✅ Yes                             | ❌ No            | Function runs even when not needed |
| With `useMemo`    | ❌ No (only on dependency change)  | ✅ Yes           | Cached value reused                |

# 🧩 Key Takeaway

- useMemo caches the result of a function (a value).
- It re-computes only when dependencies change.
- Great for expensive calculations or derived state.
# ⚡ Avoid overusing it — use only when you face actual performance issues.


## ⚛️ useCallback in React

### 🧠 Definition
`useCallback` is a React Hook that **memoizes a function**, returning the **same function reference** across re-renders — unless its dependencies change.

This helps prevent **unnecessary re-renders**, especially when passing functions as props to child components.

---

### 🔹 Example 1: useCallback With `React.memo`

```jsx
import React, { useState, useCallback } from "react";
const ChildButton= React.memo(({ onClick }) {
  console.log("Child rendered");
  return <button onClick={onClick}>Click Me</button>;
})
function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");

  // ✅ useCallback keeps the same function reference across renders
  const handleClick = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);

  return (
    <div>
      <h2>Count: {count}</h2>
      <ChildButton onClick={handleClick} />
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type..."
      />
    </div>
  );
}

export default Parent;
```
## 🧩 Explanation:
- Now the child is wrapped with React.memo, which skips re-rendering if props haven’t changed.
- Since useCallback keeps the same function reference, React.memo sees no prop change → ✅ Child does not re-render.
## SUMMARY TABLE
```
| Scenario                     | Function Reference | Child Re-renders?    | Explanation                                   |
| ---------------------------- | ------------------ | -----------------    | --------------------------------------------- |
| No optimization              | ❌ New each render  | ✅ Yes             | Every re-render creates new function          |
| Only `useCallback`           | ✅ Same             | ✅ Yes             | Function stable but React doesn’t skip render |
| Only `React.memo`            | ❌ New each render  | ✅ Yes             | Memoized component still sees prop change     |
| `React.memo` + `useCallback` | ✅ Same             | ❌ No              | Function stable and React skips re-render     |
```





## 13. How to optimize React performance?
- Use **React.memo** for memoization
- Use **useMemo / useCallback** hooks
- Code splitting with **React.lazy + Suspense**
- Avoid unnecessary re-renders
- Virtualize long lists (e.g., `react-window`)

---

## ✅ Quick Recap
- React = UI library with Virtual DOM
- Uses **components, props, state, hooks**
- For navigation → **React Router**
- For global state → **Context API / Redux**
- Performance → **memoization + code splitting**
