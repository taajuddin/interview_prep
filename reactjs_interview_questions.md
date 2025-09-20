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
