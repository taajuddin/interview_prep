# Node.js Interview Questions & Answers

This document contains frequently asked **Node.js interview questions** with simple explanations and examples.

---

## 1. What is Node.js?
- Node.js is a **runtime environment** that lets you run JavaScript code outside the browser.
- Built on **Google Chrome’s V8 engine**.
- Uses **event-driven, non-blocking I/O**.

---

## 2. Why use Node.js?
- Fast and scalable
- Single-threaded but handles many connections concurrently
- Huge ecosystem (NPM)
- Perfect for real-time apps (chat, streaming)

---

## 3. What is NPM?
- **Node Package Manager**
- Comes with Node.js installation
- Used to install/manage packages

```bash
npm init -y   # initialize project
npm install express   # install express
```

---

## 4. What are CommonJS and ES Modules?
- **CommonJS**: Uses `require`
```js
const fs = require('fs');
```
- **ES Module**: Uses `import`
```js
import fs from 'fs';
```

---

## 5. What is Event Loop in Node.js?
- A mechanism that handles **async operations**.
- Node.js is single-threaded but uses the event loop to process callbacks efficiently.

---

## 6. What is the difference between setImmediate(), setTimeout(), and process.nextTick()?
- `setImmediate()` → Executes after I/O events.
- `setTimeout(fn, 0)` → Executes after a minimum delay.
- `process.nextTick()` → Executes immediately after the current operation.

---

## 7. How does Node.js handle asynchronous operations?
- Using callbacks, promises, and async/await.

```js
// Using async/await
async function fetchData() {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts');
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

---

## 8. What is Middleware in Node.js?
- Functions that execute between request and response in Express.js.

```js
app.use((req, res, next) => {
  console.log('Request URL:', req.url);
  next();
});
```

---

## 9. What is Express.js?
- A minimal and flexible web framework for Node.js.
- Provides routing, middleware, and HTTP utilities.

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hello Node.js'));

app.listen(3000);
```

---

## 10. What are Streams in Node.js?
- Streams handle data **piece by piece** (not all at once).
- Types: Readable, Writable, Duplex, Transform.

```js
const fs = require('fs');
const readStream = fs.createReadStream('file.txt');

readStream.on('data', chunk => console.log(chunk.toString()));
```

---

## 11. What is Cluster Module in Node.js?
- Used to run multiple instances of Node.js to utilize multi-core CPUs.

```js
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isMaster) {
  os.cpus().forEach(() => cluster.fork());
} else {
  http.createServer((req, res) => res.end('Hello')).listen(3000);
}
```

---

## 12. Difference between process and thread?
- **Process** → Independent execution, separate memory.
- **Thread** → Lightweight, share memory inside a process.

Node.js uses **single-threaded event loop**, but can spawn child processes.

---

## 13. How do you handle errors in Node.js?
- Using **try/catch**, **error-first callbacks**, or **.catch() in promises**.

```js
fs.readFile('file.txt', (err, data) => {
  if (err) return console.error(err);
  console.log(data.toString());
});
```

---

## 14. Difference between synchronous and asynchronous code?
- **Synchronous** → Executes line by line, blocking.
- **Asynchronous** → Non-blocking, uses callbacks/promises.

---

## 15. What is the difference between require() and import?
- `require()` → CommonJS, can be used anywhere.
- `import` → ES Module, static and hoisted.

---

## 16. What are child processes in Node.js?
- Allow executing other programs/scripts from Node.js.

```js
const { exec } = require('child_process');
exec('ls', (err, stdout) => console.log(stdout));
```

---

## 17. How to improve Node.js performance?
- Use clustering
- Caching responses
- Load balancing
- Use async APIs
- Optimize queries (DB, Redis, etc.)

---

## 18. Difference between fork() and spawn() in Node.js?
- `spawn()` → Launches a new process with a command.
- `fork()` → Creates a new Node.js process (child process) with IPC (communication).

---

## 19. What is JWT in Node.js?
- **JSON Web Token** → Used for authentication.
- Encoded string containing payload, header, and signature.

```js
const jwt = require('jsonwebtoken');
const token = jwt.sign({ id: 1 }, 'secretKey');
```

---

## 20. Difference between Monolithic and Microservices architecture?
- **Monolithic** → All components in a single codebase.
- **Microservices** → Each service runs independently, communicates via APIs.

---

## ✅ Quick Recap
- Node.js = JavaScript runtime (V8 engine)
- Async + event-driven (non-blocking I/O)
- Key topics: Event loop, Streams, Middleware, Clusters
- Popular framework → **Express.js**
- Handles real-time, scalable applications efficiently
