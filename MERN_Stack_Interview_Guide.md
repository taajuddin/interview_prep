# 🚀 MERN Stack Interview Questions & Answers

------------------------------------------------------------------------

## **MERN Stack Basics**

### **Q1. What is the MERN stack and why is it popular?**

**Answer:**
MERN = **MongoDB, Express.js, React.js, Node.js**
- **MongoDB** → NoSQL database (stores data in JSON-like documents).
- **Express.js** → Lightweight backend web framework for Node.js.
- **React.js** → Frontend library for building dynamic UIs.
- **Node.js** → Runtime environment to run JavaScript on the server.

**Why popular?** - Full JavaScript stack (same language frontend &
backend).
- JSON everywhere (easy data exchange).
- High performance (non-blocking Node.js).
- Large ecosystem + open source.

------------------------------------------------------------------------

### **Q2. Describe the typical architecture and data flow in a MERN application.**

**Answer:**
1. **Client (React)** → Sends HTTP request (e.g., fetch API, Axios).
2. **Server (Express + Node)** → Handles routes, processes logic.
3. **Database (MongoDB)** → Stores/retrieves JSON data via Mongoose.
4. **Response** → Server sends JSON back → React updates UI.

**Flow Example:**
User submits form → React → POST `/api/users` → Express → MongoDB →
Response → React updates UI.

------------------------------------------------------------------------

### **Q3. How do the different components of the MERN stack interact?**

-   **React (frontend)** calls APIs (Axios/fetch).
-   **Express (backend)** defines routes and controllers.
-   **Node.js** executes backend logic, connects to MongoDB.
-   **MongoDB** stores application data.

------------------------------------------------------------------------

### **Q4. How do you handle authentication & authorization in MERN?**

**Answer:**
Using **JWT (JSON Web Token)**:
1. User logs in → backend verifies credentials.
2. Generate JWT with `jsonwebtoken`.
3. Send JWT to client → stored in localStorage/cookies.
4. On protected routes → client sends JWT in headers → backend verifies.

**Example:**

``` js
// Generate Token
const token = jwt.sign({ userId: user._id }, "secret", { expiresIn: "1h" });

// Middleware to verify
function auth(req, res, next) {
  const token = req.headers["authorization"];
  if (!token) return res.sendStatus(401);
  jwt.verify(token, "secret", (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
}
```

------------------------------------------------------------------------

### **Q5. Role of middleware in Express.js?**

**Answer:** Middleware are functions that run **between request and
response**.
Uses: - Logging
- Auth check
- Parsing JSON
- Error handling

**Example:**

``` js
app.use(express.json()); // body parser
app.use((req,res,next)=>{
  console.log(`${req.method} ${req.url}`);
  next();
});
```

------------------------------------------------------------------------

### **Q6. How do you deploy a MERN application?**

**Answer:**
- **Frontend (React)** → Vercel, Netlify, S3, or served by Express.
- **Backend (Express + Node)** → Render, Heroku, AWS EC2, or Docker.
- **Database (MongoDB)** → MongoDB Atlas (cloud).
- Use **reverse proxy (NGINX/PM2)** for production.

------------------------------------------------------------------------

### **Q7. Benefits of MERN over other stacks?**

-   Single language (JS) → Faster development.
-   JSON end-to-end.
-   Large ecosystem + community.
-   Scalable with microservices.

------------------------------------------------------------------------

## **MongoDB Questions**

### **Q1. What is MongoDB & how is it different from RDBMS?**

-   **MongoDB** = NoSQL, document-oriented, schema-less.
-   **RDBMS** = Tables, fixed schema, joins.
-   MongoDB stores flexible **JSON-like docs** → faster for modern apps.

------------------------------------------------------------------------

### **Q2. Key Features of MongoDB**

-   Document-oriented (BSON).
-   Flexible schema.
-   Horizontal scaling (sharding).
-   Replication for high availability.
-   Aggregation pipeline for analytics.

------------------------------------------------------------------------

### **Q3. CRUD with Mongoose**

``` js
const User = mongoose.model("User", { name: String, age: Number });

// Create
await User.create({ name: "John", age: 25 });

// Read
const users = await User.find();

// Update
await User.updateOne({ name: "John" }, { age: 26 });

// Delete
await User.deleteOne({ name: "John" });
```

------------------------------------------------------------------------

### **Q4. Sharding vs Replication**

-   **Replication** → Copies data across servers (high availability).
-   **Sharding** → Splits data across servers (scalability).

------------------------------------------------------------------------

### **Q5. Aggregation Pipeline**

Used for data transformation (like SQL GROUP BY).

``` js
db.orders.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } }
]);
```

------------------------------------------------------------------------

### **Q6. Modeling Relationships**

-   **Embedded docs** → Faster reads (e.g., user with embedded
    address).\
-   **References** → Normalize with ObjectId refs (like foreign key).
```js
// One-to-Few relationship
const userSchema = {
  name: String,
  addresses: [{
    street: String,
    city: String,
    country: String
  }]
};

// One-to-Many relationship
const userSchema = {
  name: String,
  // Reference to posts
};

const postSchema = {
  title: String,
  content: String,
  author: { 
    type: mongoose.Schema.Types.ObjectId, 
    ref: 'User' 
  }
};

// Populate references
const posts = await Post.find().populate('author');

```

------------------------------------------------------------------------

## **Express.js Questions**

### **Q1. What is Express.js?**

A minimal Node.js framework for building web APIs and apps.

------------------------------------------------------------------------

### **Q2. Routing in Express**

``` js
app.get("/users", (req, res) => res.send("All users"));
app.post("/users", (req, res) => res.send("Create user"));
```

------------------------------------------------------------------------

### **Q3. Middleware Example**

``` js
// Built-in middleware
app.use(express.json()); // Parse JSON
app.use(express.urlencoded({ extended: true })); // Parse form data

// Third-party middleware
app.use(cors());
app.use(helmet()); // Security headers
app.use(morgan('combined')); // Logging

// Custom middleware
const requestLogger = (req, res, next) => {
  console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
  next();
};
app.use(requestLogger);

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something broke!' });
});
```

------------------------------------------------------------------------

### **Q4. Error Handling**

``` js
// Async error wrapper
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Using wrapper
app.get('/users/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    const error = new Error('User not found');
    error.statusCode = 404;
    throw error;
  }
  res.json(user);
}));

// Custom error class
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
  }
}

// Global error handler
app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    error: err.message,
    stack: process.env.NODE_ENV === 'production' ? '🥞' : err.stack
  });
});
```

------------------------------------------------------------------------

### **Q5. CORS**

``` js
const cors = require("cors");
// Enable all CORS requests
app.use(cors());

// Configure CORS
app.use(cors({
  origin: ['http://localhost:3000', 'https://myapp.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  credentials: true
}));

// Route-specific CORS
app.get('/public-data', cors(), (req, res) => {
  res.json({ public: 'data' });
});
```

------------------------------------------------------------------------

## **React.js Questions**

### **Q1. What is React.js?**

A UI library for building fast, reusable component-based apps.

Core Features → Components, Virtual DOM, JSX, Hooks.

------------------------------------------------------------------------

### **Q2. Virtual DOM**

-   In-memory representation of real DOM.
-   React updates only changed parts → better performance.

------------------------------------------------------------------------

### **Q3. Components**

-   **Functional** (with hooks).
-   **Class-based** (with lifecycle).

``` js
function Hello({ name }) {
  return <h1>Hello {name}</h1>;
}
```

------------------------------------------------------------------------

### **Q4. State Management**

-   Local → `useState`, `useReducer`.
-   Global → Context API, Redux.
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
------------------------------------------------------------------------

### **Q5. Hooks Example**

``` js
// useEffect
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // Fetch user data when component mounts or userId changes
    const fetchUser = async () => {
      setLoading(true);
      const response = await fetch(`/api/users/${userId}`);
      const userData = await response.json();
      setUser(userData);
      setLoading(false);
    };
    
    fetchUser();
  }, [userId]); // Dependency array
  
  if (loading) return <div>Loading...</div>;
  return <div>{user.name}</div>;
}

// useContext
const ThemeContext = createContext();

function App() {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const { theme, setTheme } = useContext(ThemeContext);
  
  return (
    <div className={theme}>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

// Custom Hook
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });
  
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  
  return [value, setValue];
}

// Using custom hook
function Settings() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  const [language, setLanguage] = useLocalStorage('language', 'en');
  
  return (
    <div>
      <select value={theme} onChange={(e) => setTheme(e.target.value)}>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
      </select>
    </div>
  );
}
```

------------------------------------------------------------------------

### **Q6. Optimize React Performance**

-   Memoization (`React.memo`, `useMemo`).
-   Code splitting (lazy load).
-   Avoid unnecessary re-renders.
```js
// React.memo for functional components
const UserCard = React.memo(function UserCard({ user }) {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
    </div>
  );
});

// useMemo for expensive calculations
function ExpensiveComponent({ items, filter }) {
  const filteredItems = useMemo(() => {
    return items.filter(item => 
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]); // Recalculate only when dependencies change
  
  return (
    <div>
      {filteredItems.map(item => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
}

// useCallback for function stability
function ParentComponent() {
  const [count, setCount] = useState(0);
  const [value, setValue] = useState('');
  
  const handleClick = useCallback(() => {
    console.log('Count:', count);
  }, [count]); // Function only changes when count changes
  
  return (
    <div>
      <ChildComponent onClick={handleClick} />
      <input 
        value={value} 
        onChange={(e) => setValue(e.target.value)} 
      />
    </div>
  );
}

// Code splitting with React.lazy
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```
------------------------------------------------------------------------

### **Q7. Higher-Order Components (HOCs)**

A function that takes a component & returns a new one.
Used for cross-cutting concerns (e.g., auth, logging).
```js
// HOC for authentication
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
      // Check authentication
      const token = localStorage.getItem('token');
      if (token) {
        // Verify token and set user
        setUser({ name: 'John Doe' });
      }
      setLoading(false);
    }, []);
    
    if (loading) return <div>Loading...</div>;
    if (!user) return <div>Please log in</div>;
    
    return <WrappedComponent {...props} user={user} />;
  };
}

// Using HOC
function Dashboard({ user }) {
  return <div>Welcome to dashboard, {user.name}!</div>;
}

const AuthenticatedDashboard = withAuth(Dashboard);

// HOC for logging
function withLogger(WrappedComponent) {
  return function LoggedComponent(props) {
    useEffect(() => {
      console.log(`Component ${WrappedComponent.name} mounted`);
      return () => {
        console.log(`Component ${WrappedComponent.name} unmounted`);
      };
    }, []);
    
    return <WrappedComponent {...props} />;
  };
}
```
------------------------------------------------------------------------

## **Node.js Questions**

### **Q1. What is Node.js?**

JS runtime built on Chrome V8 → runs JS on server.
```js
// Why suitable for MERN?
// - JavaScript everywhere
// - Non-blocking I/O
// - Rich ecosystem (npm)
// - Great for I/O-intensive apps
// - Fast development cycle

// Simple HTTP server
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello from Node.js!');
});

server.listen(3000, () => {
  console.log('Server running on port 3000');
});
```
------------------------------------------------------------------------

### **Q2. Event Loop**

Handles async tasks (I/O, timers) → non-blocking concurrency.
```js
// Node.js uses event-driven, non-blocking I/O
console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise 1');
});

console.log('End');

// Output order:
// Start
// End
// Promise 1 (microtask)
// Timeout 1 (macrotask)
```
------------------------------------------------------------------------

### **Q3. Modules**

-   CommonJS (`require`)
-   ES Modules (`import`).
    Managed via **NPM**.
```
// Creating modules
// math.js
exports.add = (a, b) => a + b;
exports.multiply = (a, b) => a * b;

// Or
module.exports = {
  add: (a, b) => a + b,
  multiply: (a, b) => a * b
};

// Using modules
const math = require('./math.js');
console.log(math.add(2, 3)); // 5

// ES6 modules
// math.mjs
export const add = (a, b) => a + b;
export const multiply = (a, b) => a * b;

// app.mjs
import { add, multiply } from './math.mjs';

```
------------------------------------------------------------------------

### **Q4. Blocking vs Non-Blocking**

-   **Blocking** → Code waits (e.g., `fs.readFileSync`).
-   **Non-blocking** → Uses callbacks/promises (`fs.readFile`).
```js
// Blocking (Synchronous)
const fs = require('fs');

console.log('Start');
const data = fs.readFileSync('file.txt', 'utf8'); // Blocks here
console.log(data);
console.log('End');

// Non-blocking (Asynchronous)
console.log('Start');
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
console.log('End'); // This runs immediately
```
------------------------------------------------------------------------

### **Q5. Streams & Buffers**

-   **Stream** → Handles large data in chunks (video, file).
-   **Buffer** → Temporary memory storage for binary data.
```js
// Buffers - Temporary storage for binary data
const buffer = Buffer.from('Hello World', 'utf8');
console.log(buffer); // <Buffer 48 65 6c 6c 6f 20 57 6f 72 6c 64>
console.log(buffer.toString()); // Hello World

// Streams - Handle data in chunks
const fs = require('fs');

// Read stream
const readStream = fs.createReadStream('largefile.txt', 'utf8');
readStream.on('data', (chunk) => {
  console.log('Received chunk:', chunk.length);
});

readStream.on('end', () => {
  console.log('Finished reading');
});

// Write stream
const writeStream = fs.createWriteStream('output.txt');
writeStream.write('Hello ');
writeStream.write('World!');
writeStream.end();

// Piping streams
readStream.pipe(writeStream);

```

------------------------------------------------------------------------

## **Scenario-Based & Advanced**

### **Q1. Real-time Data**

Use **WebSockets (Socket.IO)** for chat, notifications.
```js
// server.js (or index.js)

// 1. Setup Express and HTTP Server
const express = require('express');
const http = require('http');
const { Server } = require('socket.io'); // Import the Socket.IO Server
const app = express();

// Create a standard Node.js HTTP server instance
const server = http.createServer(app); 

// 2. Configure Socket.IO
// Attach Socket.IO to the HTTP server.
// 'cors' is crucial for connecting a frontend (e.g., React on port 3000)
const io = new Server(server, {
  cors: {
    origin: "http://localhost:3000", // Allow connection from React dev server
    methods: ["GET", "POST"]
  }
});

// 3. Socket.IO Connection Logic
// This block handles new client connections and real-time events.
io.on('connection', (socket) => {
  const userId = socket.id.substring(0, 4); // Simple unique ID for demo

  console.log(`User connected: ${userId}`);

  // 3a. Handle a 'chat message' event from a client
  socket.on('chat message', (msg) => {
    // Log the message on the server
    console.log(`Message from ${userId}: ${msg}`);
    
    // 3b. Broadcast the message to ALL connected clients
    // 'io.emit' sends the data to everyone, including the sender
    io.emit('chat message', {
      user: userId,
      text: msg,
      timestamp: new Date().toLocaleTimeString()
    });
  });

  // 3c. Handle 'notification' logic (e.g., when a user joins or disconnects)
  socket.broadcast.emit('notification', `${userId} has joined the chat.`);
  // 'socket.broadcast.emit' sends the data to everyone EXCEPT the sender

  // 3d. Handle disconnection
  socket.on('disconnect', () => {
    console.log(`User disconnected: ${userId}`);
    io.emit('notification', `${userId} has left the chat.`);
  });
});

// 4. Start the Server
const PORT = process.env.PORT || 5000;
server.listen(PORT, () => { // Use 'server.listen' NOT 'app.listen'
  console.log(`Server running on port ${PORT}`);
});
```

------------------------------------------------------------------------

### **Q2. Scaling**

-   Horizontal scaling with load balancers.
-   Caching (Redis).
-   Database sharding.

  ```js
// Strategies:
// 1. Load Balancing
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
  const numCPUs = os.cpus().length;
  
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
  
  cluster.on('exit', (worker) => {
    console.log(`Worker ${worker.process.pid} died`);
    cluster.fork();
  });
} else {
  // Worker process
  app.listen(3000, () => {
    console.log(`Worker ${process.pid} started`);
  });
}

// 2. Database Optimization
// - Indexing
userSchema.index({ email: 1 }); // Create index
userSchema.index({ createdAt: -1 }); // Descending index

// - Query optimization
// Use projection to select only needed fields
User.find({}, 'name email'); // Only name and email

// 3. Caching with Redis
const redis = require('redis');
const client = redis.createClient();

async function getUsers() {
  const cacheKey = 'all_users';
  
  // Check cache first
  const cached = await client.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }
  
  // If not in cache, fetch from database
  const users = await User.find();
  
  // Store in cache for 5 minutes
  await client.setex(cacheKey, 300, JSON.stringify(users));
  
  return users;
}
```

------------------------------------------------------------------------

### **Q3. Securing MERN App**

-   Use HTTPS.
-   Store passwords with bcrypt.
-   Use JWT securely (short expiry, refresh tokens).
-   Sanitize inputs (prevent SQL/NoSQL injection, XSS).
```js
// 1. Input Validation
const Joi = require('joi');

const userSchema = Joi.object({
  name: Joi.string().min(2).max(50).required(),
  email: Joi.string().email().required(),
  password: Joi.string().min(8).pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
});

app.post('/api/register', async (req, res) => {
  const { error } = userSchema.validate(req.body);
  if (error) {
    return res.status(400).json({ error: error.details[0].message });
  }
  // Process valid data
});

// 2. Helmet for security headers
app.use(helmet());

// 3. Rate limiting
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP'
});

app.use('/api/', limiter);

// 4. SQL Injection prevention (MongoDB is generally safe)
// Use parameterized queries with Mongoose

// 5. XSS protection
app.use(helmet.xssFilter());

// 6. CSRF protection
const csrf = require('csurf');
app.use(csrf({ cookie: true }));
```
------------------------------------------------------------------------

### **Q4. File Uploads**

Use **Multer (Express middleware)** → store in server or AWS S3.
```js
// Backend - Multer for file uploads
const multer = require('multer');
const path = require('path');

// Configure storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const uniqueName = Date.now() + '-' + Math.round(Math.random() * 1E9);
    cb(null, uniqueName + path.extname(file.originalname));
  }
});

// File filter
const fileFilter = (req, file, cb) => {
  if (file.mimetype.startsWith('image/')) {
    cb(null, true);
  } else {
    cb(new Error('Only image files are allowed'), false);
  }
};

const upload = multer({ 
  storage, 
  fileFilter,
  limits: { fileSize: 5 * 1024 * 1024 } // 5MB limit
});

// Upload route
app.post('/api/upload', upload.single('image'), async (req, res) => {
  try {
    if (!req.file) {
      return res.status(400).json({ error: 'No file uploaded' });
    }
    
    // Save file info to database
    const fileRecord = new File({
      filename: req.file.filename,
      originalName: req.file.originalname,
      path: req.file.path,
      size: req.file.size,
      uploadedBy: req.user.id
    });
    
    await fileRecord.save();
    
    res.json({
      message: 'File uploaded successfully',
      file: {
        id: fileRecord._id,
        filename: fileRecord.filename,
        url: `/uploads/${fileRecord.filename}`
      }
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Frontend - React file upload
function FileUpload() {
  const [uploading, setUploading] = useState(false);
  const [selectedFile, setSelectedFile] = useState(null);
  
  const handleFileSelect = (event) => {
    setSelectedFile(event.target.files[0]);
  };
  
  const handleUpload = async () => {
    if (!selectedFile) return;
    
    setUploading(true);
    const formData = new FormData();
    formData.append('image', selectedFile);
    
    try {
      const response = await fetch('/api/upload', {
        method: 'POST',
        body: formData,
        headers: {
          'Authorization': `Bearer ${localStorage.getItem('token')}`
        }
      });
      
      const result = await response.json();
      console.log('Upload success:', result);
    } catch (error) {
      console.error('Upload failed:', error);
    } finally {
      setUploading(false);
    }
  };
  
  return (
    <div>
      <input type="file" onChange={handleFileSelect} />
      <button onClick={handleUpload} disabled={uploading || !selectedFile}>
        {uploading ? 'Uploading...' : 'Upload'}
      </button>
    </div>
  );
}
```

------------------------------------------------------------------------

### **Q5. Testing Approaches**

-   **Unit** → Jest, Mocha.
-   **Integration** → Supertest (API testing).
-   **E2E** → Cypress, Playwright.
```js
// Backend Testing - Jest + Supertest
// __tests__/user.test.js
const request = require('supertest');
const app = require('../app');
const User = require('../models/User');

describe('User API', () => {
  beforeEach(async () => {
    await User.deleteMany({});
  });
  
  test('should create a new user', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({
        name: 'John Doe',
        email: 'john@test.com',
        password: 'password123'
      })
      .expect(201);
    
    expect(response.body.user.name).toBe('John Doe');
    expect(response.body.user.email).toBe('john@test.com');
  });
  
  test('should get all users', async () => {
    await User.create({
      name: 'Jane Doe',
      email: 'jane@test.com',
      password: 'password123'
    });
    
    const response = await request(app)
      .get('/api/users')
      .expect(200);
    
    expect(response.body.length).toBe(1);
    expect(response.body[0].name).toBe('Jane Doe');
  });
});

// Frontend Testing - React Testing Library
// __tests__/UserComponent.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import UserComponent from '../UserComponent';

test('renders user information', () => {
  const user = { name: 'John Doe', email: 'john@test.com' };
  
  render(<UserComponent user={user} />);
  
  expect(screen.getByText('John Doe')).toBeInTheDocument();
  expect(screen.getByText('john@test.com')).toBeInTheDocument();
});

test('handles button click', () => {
  const mockOnClick = jest.fn();
  
  render(<UserComponent user={{}} onEdit={mockOnClick} />);
  
  fireEvent.click(screen.getByText('Edit'));
  
  expect(mockOnClick).toHaveBeenCalledTimes(1);
});

// Integration Testing
describe('User Registration Flow', () => {
  test('complete user registration', async () => {
    // 1. Fill registration form
    render(<RegistrationForm />);
    
    fireEvent.change(screen.getByLabelText(/name/i), {
      target: { value: 'John Doe' }
    });
    
    fireEvent.change(screen.getByLabelText(/email/i), {
      target: { value: 'john@test.com' }
    });
    
    // 2. Submit form
    fireEvent.click(screen.getByText('Register'));
    
    // 3. Verify success message
    await screen.findByText('Registration successful!');
  });
});

// E2E Testing - Cypress
// cypress/integration/user.spec.js
describe('User Management', () => {
  it('should register a new user', () => {
    cy.visit('/register');
    
    cy.get('[data-testid=name]').type('John Doe');
    cy.get('[data-testid=email]').type('john@test.com');
    cy.get('[data-testid=password]').type('password123');
    cy.get('[data-testid=register-btn]').click();
    
    cy.url().should('include', '/dashboard');
    cy.contains('Welcome, John Doe');
  });
});
```
------------------------------------------------------------------------

✅ This covers **MERN + MongoDB + Express + React + Node + Advanced
Scenarios** with examples.
