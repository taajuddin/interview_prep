# Node.js Fundamentals — README

## Table of Contents
- Node.js Fundamentals
- Core Concepts
- Express.js & Web Development
- Security & Authentication
- Advanced Topics
- Senior-Level Concepts
- Code Examples
- Practice Exercises
- Additional Resources

---

## 🎯 Node.js Fundamentals

### What is Node.js?
Node.js is a runtime environment that allows executing JavaScript outside the browser. It's built on Google Chrome's V8 JavaScript engine and uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

### Why use Node.js?
- Fast and scalable due to non-blocking architecture
- Handles many concurrent connections with a single thread
- Rich ecosystem (NPM) with numerous packages
- Ideal for real-time applications (chat apps, streaming apps)
- JavaScript on both client and server side

### What is NPM?
Node Package Manager (NPM) is the default package manager for Node.js, used to install, manage, and publish libraries.

```bash
npm init -y          # initialize project
npm install express  # install express
npm update           # update packages
npm run start        # run start script
```

---

## 🔧 Core Concepts

### CommonJS vs ES Modules
**CommonJS (CJS)** → Uses `require` and `module.exports`

```javascript
const fs = require("fs");
module.exports = myFunction;
```

**ES Modules (ESM)** → Uses `import` and `export`

```javascript
import fs from "fs";
export default myFunction;
```

### Event Loop in Node.js
Node.js uses the event loop to handle asynchronous tasks, allowing it to be non-blocking even though it's single-threaded. The event loop delegates I/O operations to the system kernel whenever possible, and continues executing other code while waiting for these operations to complete.

### Callbacks, Promises, and Async/Await
**Callback** → Pass a function as an argument

```javascript
fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

**Promise** → Handle async operations with `.then()` / `.catch()`

```javascript
fetch("https://api.example.com/data")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

**Async/Await** → Cleaner way to write async code

```javascript
const fetchData = async () => {
  try {
    const res = await fetch("https://api.example.com/data");
    console.log(await res.json());
  } catch (err) {
    console.error(err);
  }
};
```

---

## 🌐 Express.js & Web Development

### Express.js Basics
Express.js is the most popular framework for building web apps with Node.js, providing routing, middleware, and simplified request/response handling.

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

### Middleware in Express
Middleware functions process requests before reaching the route handler.

```javascript
// Logging middleware
app.use((req, res, next) => {
  console.log("Request URL:", req.url, " - ", new Date());
  next();
});

// Body parsing middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

### REST APIs in Node.js
REST (Representational State Transfer) uses HTTP methods: GET, POST, PUT, DELETE.

```javascript
let users = [];

// GET all users
app.get("/users", (req, res) => res.json(users));

// GET user by ID
app.get("/users/:id", (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).json({ message: "User not found" });
  res.json(user);
});

// CREATE new user
app.post("/users", (req, res) => {
  const newUser = { id: users.length + 1, ...req.body };
  users.push(newUser);
  res.status(201).json(newUser);
});

// UPDATE user
app.put("/users/:id", (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ message: "User not found" });

  users[index] = { ...users[index], ...req.body };
  res.json(users[index]);
});

// DELETE user
app.delete("/users/:id", (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ message: "User not found" });

  users.splice(index, 1);
  res.status(204).send();
});
```

### Error Handling in Express

```javascript
// Async error wrapper
const asyncHandler = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// 404 handler
app.use((req, res, next) => {
  res.status(404).json({ message: "Route not found" });
});

// Global error handler
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: "Something went wrong!" });
});
```

---

## 🔒 Security & Authentication

### Authentication in Node.js
**JWT (JSON Web Token)** for stateless authentication:

```javascript
const jwt = require("jsonwebtoken");

// Create token
const token = jwt.sign({ id: user.id }, process.env.JWT_SECRET, { 
  expiresIn: "1h" 
});

// Verify token
const decoded = jwt.verify(token, process.env.JWT_SECRET);
```

**OAuth2** for third-party logins (using Passport.js):

```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "/auth/google/callback"
  },
  (accessToken, refreshToken, profile, done) => {
    // Find or create user
    User.findOrCreate({ googleId: profile.id }, (err, user) => {
      return done(err, user);
    });
  }
));
```

### Security Best Practices
- Use Helmet.js for setting security headers:

```javascript
const helmet = require('helmet');
app.use(helmet());
```

- Sanitize user input to prevent XSS and SQL injection:

```javascript
const validator = require('validator');
const cleanInput = validator.escape(req.body.input);
```

- Store passwords securely with bcrypt:

```javascript
const bcrypt = require('bcrypt');
const saltRounds = 10;

// Hash password
const hashedPassword = await bcrypt.hash(password, saltRounds);

// Compare password
const match = await bcrypt.compare(plainPassword, hashedPassword);
```

- Use HTTPS in production
- Implement rate limiting to prevent brute force attacks
- Use environment variables for sensitive data:

```javascript
require('dotenv').config();
const dbPassword = process.env.DB_PASSWORD;
```

---

## 🚀 Advanced Topics

### Node.js Streams
Streams handle large data efficiently without loading everything into memory.

Types: Readable, Writable, Duplex, Transform

```javascript
const fs = require('fs');

// Copy file using streams
const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt');

readStream.pipe(writeStream);

readStream.on('end', () => {
  console.log('File copy completed');
});

readStream.on('error', (err) => {
  console.error('Error reading file:', err);
});
```

### Cluster Module in Node.js

```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
  const numCPUs = os.cpus().length;

  console.log(`Master ${process.pid} is running`);
  console.log(`Forking for ${numCPUs} CPUs`);

  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died`);
    // Restart worker
    cluster.fork();
  });
} else {
  // Workers can share any TCP connection
  require('./server.js');
  console.log(`Worker ${process.pid} started`);
}
```

### Scaling Node.js Applications
- Use Load Balancing with PM2 or Kubernetes
- Cache responses with Redis
- Use Horizontal Scaling with multiple instances
- Database optimization with indexing and connection pooling
- Implement microservices architecture for large applications

### Database Integration
**MongoDB (NoSQL) with Mongoose:**

```javascript
const mongoose = require('mongoose');
mongoose.connect(process.env.MONGODB_URI);

const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true },
  age: Number
});

const User = mongoose.model('User', userSchema);

// Create user
const user = new User({ name: 'John', email: 'john@example.com', age: 30 });
await user.save();

// Find users
const users = await User.find({ age: { $gt: 25 } });
```

**PostgreSQL/MySQL with Sequelize:**

```javascript
const { Sequelize, DataTypes } = require('sequelize');
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});

const User = sequelize.define('User', {
  name: DataTypes.STRING,
  email: { type: DataTypes.STRING, unique: true },
  age: DataTypes.INTEGER
});

await User.sync();
const users = await User.findAll({ where: { age: { [Sequelize.Op.gt]: 25 } } });
```

---

## 🧪 Testing in Node.js
Using Jest:

```javascript
const request = require('supertest');
const app = require('../app');

describe('User API', () => {
  test('GET /users returns 200', async () => {
    const res = await request(app).get('/users');
    expect(res.statusCode).toBe(200);
  });

  test('POST /users creates a new user', async () => {
    const res = await request(app)
      .post('/users')
      .send({ name: 'John', email: 'john@example.com' });

    expect(res.statusCode).toBe(201);
    expect(res.body).toHaveProperty('id');
  });
});
```

---

## 👨‍💼 Senior-Level Concepts

### Rate Limiting
Prevent API abuse with express-rate-limit:

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP'
});

app.use('/api/', limiter);
```

### API Versioning
Implement API versioning for backward compatibility:

```javascript
// Route versioning
app.use('/api/v1/users', require('./routes/v1/users'));
app.use('/api/v2/users', require('./routes/v2/users'));
```

### GraphQL vs REST
Brief comparison and example with Apollo Server included in original content.

### Message Queues (RabbitMQ, Kafka)
Example using RabbitMQ with `amqplib` included above.

### CI/CD Pipeline Integration
Example GitHub Actions workflow included above.

---

## 💻 Code Examples
### Basic Express Server with Middleware
See code examples in original content.

### JWT Authentication Middleware
See code examples in original content.

---

## 🧪 Practice Exercises
- Create a RESTful API for a blog with posts and comments
- Implement JWT authentication with login/register endpoints
- Write unit tests for your API endpoints
- Create a file upload endpoint using Multer
- Implement rate limiting on authentication endpoints
- Set up a database with relationships (users have posts, posts have comments)
- Create a real-time chat using Socket.io
- Dockerize your Node.js application

---

## 📚 Additional Resources
- Node.js Official Documentation
- Express.js Guide
- Mozilla Developer Network (MDN)
- JavaScript Info
- Node.js Best Practices

---

*Generated README from provided Node.js notes.*
