# Redux Interview Questions & Answers

This document contains frequently asked **Redux interview questions** with simple and clear explanations.

---

## 1. What is Redux?
Redux is a **state management library** for JavaScript applications. It helps manage application state in a predictable way using a **single source of truth (store)**.

---

## 2. Why do we use Redux?
- Centralized state management
- Predictable behavior
- Easier debugging
- Consistent state across components
- Better for large-scale applications

---

## 3. What are the core principles of Redux?
1. **Single Source of Truth** → The state of your application is stored in one object.
2. **State is Read-only** → The only way to change the state is by dispatching an action.
3. **Changes using Pure Functions** → Reducers specify how the state changes based on actions.

---

## 4. What are Actions in Redux?
- **Plain JavaScript objects** that describe what happened.
- Must have a `type` property.

```js
const addTodo = { type: "ADD_TODO", payload: "Learn Redux" };
```

---

## 5. What are Reducers?
- **Pure functions** that take the current state and an action, and return the new state.

```js
function todoReducer(state = [], action) {
  switch (action.type) {
    case "ADD_TODO":
      return [...state, action.payload];
    default:
      return state;
  }
}
```

---

## 6. What is a Redux Store?
- A single object that holds the **application state**.
- Created using `createStore()`.

```js
import { createStore } from "redux";
const store = createStore(todoReducer);
```

---

## 7. What is Middleware in Redux?
- Middleware lets you **extend Redux** with custom functionality.
- Common example: **redux-thunk** for async actions.

```js
const logger = store => next => action => {
  console.log("Dispatching:", action);
  return next(action);
};
```

---

## 8. What is Redux Thunk?
- A middleware that allows writing **async logic** (like API calls) inside action creators.

```js
const fetchUsers = () => async (dispatch) => {
  dispatch({ type: "FETCH_USERS_REQUEST" });
  const res = await fetch("https://jsonplaceholder.typicode.com/users");
  const data = await res.json();
  dispatch({ type: "FETCH_USERS_SUCCESS", payload: data });
};
```

---

## 9. Difference between Redux and Context API?
| Feature            | Redux | Context API |
|--------------------|-------|-------------|
| Purpose            | State management | Prop drilling solution |
| Boilerplate        | More   | Less        |
| Async handling     | Yes (middlewares) | No |
| Scalability        | Best for large apps | Small to medium apps |

---

## 10. What are Redux Toolkit and its benefits?
- **Redux Toolkit (RTK)** is the official, recommended way to write Redux logic.
- Provides utilities like `createSlice`, `configureStore`, and `createAsyncThunk`.
- Reduces boilerplate and simplifies Redux setup.

```js
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1 },
    decrement: (state) => { state.value -= 1 }
  }
});
```

---

## 11. How do you connect Redux with React?
- Using `react-redux` package.
- `Provider` makes the store available.
- `useSelector` to read state, `useDispatch` to dispatch actions.

```js
import { Provider, useSelector, useDispatch } from "react-redux";

function Counter() {
  const count = useSelector(state => state.counter.value);
  const dispatch = useDispatch();

  return (
    <>
      <p>{count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>
    </>
  );
}
```

---
## context API todo App
```js
// useState Hook
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

// useReducer Hook
function todoReducer(state, action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, { id: Date.now(), text: action.text }];
    case 'REMOVE_TODO':
      return state.filter(todo => todo.id !== action.id);
    default:
      return state;
  }
}

function TodoList() {
  const [todos, dispatch] = useReducer(todoReducer, []);
  const [input, setInput] = useState('');
  
  const addTodo = () => {
    dispatch({ type: 'ADD_TODO', text: input });
    setInput('');
  };
  
  return (
    <div>
      <input 
        value={input} 
        onChange={(e) => setInput(e.target.value)} 
      />
      <button onClick={addTodo}>Add Todo</button>
      {todos.map(todo => (
        <div key={todo.id}>
          {todo.text}
          <button onClick={() => 
            dispatch({ type: 'REMOVE_TODO', id: todo.id })
          }>
            Remove
          </button>
        </div>
      ))}
    </div>
  );
}

// Context API
const UserContext = createContext();

function App() {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Navbar />
      <Profile />
    </UserContext.Provider>
  );
}

function Profile() {
  const { user } = useContext(UserContext);
  return <div>Welcome, {user?.name}</div>;
}

```
---
## Redux with toolkit and plain redux example
---
```js
// redux.js   with toolkit
import { configureStore, createSlice } from "@reduxjs/toolkit";

const todoSlice = createSlice({
  name: "todos",
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      state.push({
        id: Date.now(),
        text: action.payload,
        completed: false
      });
    },
    toggleTodo: (state, action) => {
      const todo = state.find((t) => t.id === action.payload);
      if (todo) todo.completed = !todo.completed;
    },
    deleteTodo: (state, action) => {
      return state.filter((t) => t.id !== action.payload);
    }
  }
});

export const { addTodo, toggleTodo, deleteTodo } = todoSlice.actions;

export const store = configureStore({
  reducer: {
    todos: todoSlice.reducer,
  },
});

```
```js
// redux.js  pure redux 
import { createStore } from "redux";

// ACTION TYPES
const ADD_TODO = "ADD_TODO";
const TOGGLE_TODO = "TOGGLE_TODO";
const DELETE_TODO = "DELETE_TODO";

// ACTION CREATORS
export const addTodo = (text) => ({
  type: ADD_TODO,
  payload: text,
});

export const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id,
});

export const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: id,
});

// INITIAL STATE
const initialState = {
  todos: [],
};

// REDUCER (PURE FUNCTION)
function todoReducer(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return {
        todos: [
          ...state.todos,
          { id: Date.now(), text: action.payload, completed: false },
        ],
      };

    case TOGGLE_TODO:
      return {
        todos: state.todos.map((t) =>
          t.id === action.payload ? { ...t, completed: !t.completed } : t
        ),
      };

    case DELETE_TODO:
      return {
        todos: state.todos.filter((t) => t.id !== action.payload),
      };

    default:
      return state;
  }
}

// CREATE STORE
export const store = createStore(todoReducer);

```
```js
// App.jsx compoent
import React, { useState } from "react";
import { useSelector, useDispatch, Provider } from "react-redux";
import { store, addTodo, toggleTodo, deleteTodo } from "./redux";

function App() {
  const [input, setInput] = useState("");
  const todos = useSelector((state) => state.todos);
  const dispatch = useDispatch();

  const handleAdd = () => {
    if (!input.trim()) return;
    dispatch(addTodo(input));
    setInput("");
  };

  return (
    <div style={{ width: 400, margin: "50px auto", textAlign: "center" }}>
      <h2>Redux Todo App (2 Files)</h2>

      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Add todo..."
        style={{ padding: 8, width: "70%" }}
      />

      <button
        onClick={handleAdd}
        style={{ padding: "8px 12px", marginLeft: 10 }}
      >
        Add
      </button>

      <ul style={{ textAlign: "left", marginTop: 20 }}>
        {todos.map((todo) => (
          <li key={todo.id} style={{ marginBottom: 10 }}>
            <span
              onClick={() => dispatch(toggleTodo(todo.id))}
              style={{
                textDecoration: todo.completed ? "line-through" : "none",
                cursor: "pointer",
                marginRight: 10
              }}
            >
              {todo.text}
            </span>
            <button onClick={() => dispatch(deleteTodo(todo.id))}>❌</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

// Wrap App with Provider
export default function Root() {
  return (
    <Provider store={store}>
      <App />
    </Provider>
  );
}
```
---

---

## 12. Common Redux Interview Tricky Questions
- Is Redux synchronous or asynchronous? → **By default synchronous, async via middleware**.
- Can Redux work without React? → **Yes, it’s a JS library, not React-specific**.
- Is Redux always necessary? → **No, only for large-scale apps with complex state**.

---

## ✅ Quick Recap
- Redux = predictable state management
- Uses **store, actions, reducers**
- Async → Middleware like **Thunk / Saga**
- Modern way → **Redux Toolkit (RTK)**
