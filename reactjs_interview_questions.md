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
| Scenario                     | Function Reference | Child Re-renders? | Explanation                                   |
| ---------------------------- | ------------------ | ----------------- | --------------------------------------------- |
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

### **What is React? Describe its core benefits.**
React is a **JavaScript library** for building **user interfaces**, mainly **single-page applications (SPAs)**. It uses a **component-based architecture** and **Virtual DOM** for efficient rendering.

**Core benefits:**
- Fast rendering (Virtual DOM)
- Reusable components
- One-way data flow
- Strong community support
---

### **Difference between React Node, React Element, and React Component**
| Term | Description |
|------|--------------|
| **React Element** | Object describing what to render (via JSX or `React.createElement`) |
| **React Node** | Anything React can render — string, number, element, etc. |
| **React Component** | Function or class returning React elements |

```jsx
const element = <div>Hello</div>; // React Element
const node = "Hello"; // React Node
function MyComp() { return <div>Hi</div>; } // React Component
```

---

### **What is JSX, and how does it get compiled?**
JSX is **syntactic sugar** for `React.createElement()`. It’s compiled by Babel into JavaScript objects.

```jsx
const element = <h1>Hello</h1>;
// Compiled to:
const element = React.createElement('h1', null, 'Hello');
```

---

### **Difference between state and props**
| State | Props |
|-------|--------|
| Internal, mutable data | External, read-only data |
| Managed inside component | Passed from parent |
| Updated with `setState` or `useState` | Immutable |

```jsx
function Child({ name }) { return <p>{name}</p>; }
function Parent() {
  const [name, setName] = useState('Taaj');
  return <Child name={name} />;
}
```

---

### **Why do we use the key prop in React lists?**
Keys help React identify which items changed, improving re-render efficiency.

```jsx
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

---

### **What happens when you use array indices as keys?**
It can cause **incorrect re-renders** and **UI inconsistencies** when the list order changes.

---

### **7️⃣ Controlled vs Uncontrolled Components**
| Controlled | Uncontrolled |
|-------------|--------------|
| Managed by React state | Managed by the DOM |
| Uses `value` + `onChange` | Uses `defaultValue` + `ref` |

```jsx
// Controlled
<input value={name} onChange={e => setName(e.target.value)} />

// Uncontrolled
<input defaultValue="Taaj" ref={inputRef} />
```

---

### **What are the pitfalls of Context API?**
- Triggers re-renders for all consumers on value change  
- Harder to debug  
- May be overused instead of simpler prop drilling

---

### **Why were React Hooks introduced?**
Hooks allow **state and lifecycle** logic in **functional components**, improving code reuse and readability.

---

### **Rules of Hooks and why are they important**
**Rules:**
1. Call Hooks **at the top level** (not inside loops or conditions)
2. Call Hooks **only inside React components or custom Hooks**

They ensure consistent Hook ordering during renders.

---

### **useEffect vs useLayoutEffect**
| useEffect | useLayoutEffect |
|------------|----------------|
| Runs **after paint** | Runs **before paint** |
| Non-blocking | Blocking |
| Used for async tasks | Used for layout measurement |

```jsx
useEffect(() => console.log('Async side effect'));
useLayoutEffect(() => console.log('Synchronous layout fix'));
```

---

### **Why use the callback form of setState()?**
To avoid **stale state** when the new state depends on the previous value.

```jsx
setCount(prev => prev + 1);
```

---

### **How does the dependency array in useEffect work?**
React re-runs the effect only when dependencies change.

```jsx
useEffect(() => console.log('Runs when count changes'), [count]);
```

---

### **When to use useRef vs state**
| useRef | state |
|--------|--------|
| Mutable value without re-render | Triggers re-render |

```jsx
const ref = useRef(0); // Won't trigger re-render
const [count, setCount] = useState(0); // Will trigger
```

---

### **Difference between useCallback and useMemo**
| Hook | Returns | Used for |
|------|----------|----------|
| useCallback | Memoized function | Prevents function recreation |
| useMemo | Memoized value | Expensive calculations |

```jsx
const memoFn = useCallback(() => doSomething(count), [count]);
const memoValue = useMemo(() => heavyCalc(count), [count]);
```

---

### **When to use useReducer instead of useState**
Use for **complex state logic** or **multiple sub-states**.

```jsx
function reducer(state, action) {
  switch(action.type) {
    case 'inc': return { count: state.count + 1 };
  }
}
const [state, dispatch] = useReducer(reducer, { count: 0 });
```

---

### **What does useId do in React 18+?**
Generates a unique, **consistent ID** across client and server for accessibility and forms.

```jsx
const id = useId();
<label htmlFor={id}>Name</label>
<input id={id} />
```

---

### **What triggers a re-render in React?**
- State changes  
- Props changes  
- Parent re-renders  
- Context value updates

---

### **What is Reconciliation and how does React handle it efficiently?**
Reconciliation is React’s **diffing algorithm** comparing Virtual DOM trees to update only changed parts of the real DOM.

---

### **What are React Fragments, and why should you use them?**
Fragments let you group elements without extra DOM nodes.

```jsx
return (
  <>
    <h1>Title</h1>
    <p>Subtitle</p>
  </>
);
```

---

## 🧩 Advanced Topics

### **Error Boundaries & Logging**
Error Boundaries catch **JavaScript errors** in React components and display fallback UIs instead of crashing.

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  componentDidCatch(error, info) { console.error(error, info); }
  render() { return this.state.hasError ? <h2>Something went wrong.</h2> : this.props.children; }
}
```
Wrap components:
```jsx
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

---

### **Micro-Frontends & Module Federation**
**Micro-Frontends** split a large app into smaller, independently deployable apps.

**Module Federation (Webpack 5):** allows sharing React components between apps dynamically.

```js
// webpack.config.js
new ModuleFederationPlugin({
  name: 'app1',
  remotes: { app2: 'app2@http://localhost:3002/remoteEntry.js' },
  exposes: { './Header': './src/Header' }
});
```

---

### **Security in React SPAs**
**Common practices:**
- Escape user input to prevent **XSS**
- Use `dangerouslySetInnerHTML` only when necessary
- Implement **JWT** securely (in HttpOnly cookies)
- Validate API inputs server-side
- Use **Helmet** for setting security headers

```jsx
import { Helmet } from 'react-helmet';
<Helmet>
  <meta httpEquiv="Content-Security-Policy" content="default-src 'self'" />
</Helmet>
```

---

### **A/B Testing Integration**
A/B testing involves showing different UI variants and tracking conversions.

**Approaches:**
- Integrate tools like **Optimizely**, **Google Optimize**, or **LaunchDarkly**
- Use feature flags to conditionally render components

```jsx
{featureFlag ? <NewButton /> : <OldButton />}
```

---

## ⚙️ Practical Implementations

### **Tic Tac Toe App**
Tests **state management**, **conditional rendering**, and **winning logic**.

```jsx
function TicTacToe() {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [isX, setIsX] = useState(true);

  const handleClick = (i) => {
    if (board[i] || calculateWinner(board)) return;
    const newBoard = [...board];
    newBoard[i] = isX ? 'X' : 'O';
    setBoard(newBoard);
    setIsX(!isX);
  };

  const winner = calculateWinner(board);

  return (
    <>
      <h2>{winner ? `${winner} Wins!` : `Next Player: ${isX ? 'X' : 'O'}`}</h2>
      <div className="grid grid-cols-3 w-40">
        {board.map((v, i) => (
          <button key={i} onClick={() => handleClick(i)} className="border h-12 w-12 text-xl">{v}</button>
        ))}
      </div>
    </>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0,1,2], [3,4,5], [6,7,8],
    [0,3,6], [1,4,7], [2,5,8],
    [0,4,8], [2,4,6]
  ];
  for (let [a,b,c] of lines) if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) return squares[a];
  return null;
}
```

---

### **Timer App**
Tests **useEffect**, **cleanup**, and **state intervals**.

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);
  const [running, setRunning] = useState(false);

  useEffect(() => {
    let interval;
    if (running) {
      interval = setInterval(() => setSeconds(s => s + 1), 1000);
    }
    return () => clearInterval(interval);
  }, [running]);

  return (
    <>
      <h1>{seconds}s</h1>
      <button onClick={() => setRunning(true)}>Start</button>
      <button onClick={() => setRunning(false)}>Stop</button>
      <button onClick={() => setSeconds(0)}>Reset</button>
    </>
  );
}
```

---

### **Todo App**
Simple example for **CRUD operations** with React state.

```jsx
function Todo() {
  const [tasks, setTasks] = useState([]);
  const [input, setInput] = useState('');

  const addTask = () => setTasks([...tasks, input]);

  return (
    <>
      <input value={input} onChange={e => setInput(e.target.value)} />
      <button onClick={addTask}>Add</button>
      <ul>{tasks.map((t, i) => <li key={i}>{t}</li>)}</ul>
    </>
  );
}
```

---

### **Pagination or Infinite Scroll**
Used for **data-heavy lists** (APIs, dashboards, feeds).

**Pagination Example:**
```jsx
const [page, setPage] = useState(1);
useEffect(() => fetchData(page), [page]);
```

**Infinite Scroll Example:**
```jsx
useEffect(() => {
  const handleScroll = () => {
    if (window.innerHeight + window.scrollY >= document.body.offsetHeight)
      loadMore();
  };
  window.addEventListener('scroll', handleScroll);
  return () => window.removeEventListener('scroll', handleScroll);
}, []);
```

---

### **Autocomplete Search**
Tests understanding of **debouncing**, **API calls**, and **conditional rendering**.

```jsx
function Search() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  useEffect(() => {
    const timeout = setTimeout(() => fetch(`/api?q=${query}`).then(r => r.json()).then(setResults), 300);
    return () => clearTimeout(timeout);
  }, [query]);

  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <ul>{results.map(r => <li key={r.id}>{r.name}</li>)}</ul>
    </>
  );
}
```

---

### **Custom Promise.all or EventEmitter**
Shows mastery of **asynchronous logic** and **custom event management**.

**Custom Promise.all:**
```js
Promise.myAll = function(promises) {
  return new Promise((resolve, reject) => {
    let results = [], completed = 0;
    promises.forEach((p, i) => {
      Promise.resolve(p).then(val => {
        results[i] = val;
        if (++completed === promises.length) resolve(results);
      }).catch(reject);
    });
  });
};
```

**Custom EventEmitter:**
```js
class EventEmitter {
  constructor() { this.events = {}; }
  on(event, listener) { (this.events[event] ||= []).push(listener); }
  emit(event, ...args) { (this.events[event] || []).forEach(fn => fn(...args)); }
}
```

---


## ✅ Quick Recap
- React = UI library with Virtual DOM
- Uses **components, props, state, hooks**
- For navigation → **React Router**
- For global state → **Context API / Redux**
- Performance → **memoization + code splitting**
