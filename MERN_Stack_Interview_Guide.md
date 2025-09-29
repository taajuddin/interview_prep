# 🚀 MERN Stack Interview Questions & Answers

------------------------------------------------------------------------

## **MERN Stack Basics**

### **Q1. What is the MERN stack and why is it popular?**

**Answer:**\
MERN = **MongoDB, Express.js, React.js, Node.js**\
- **MongoDB** → NoSQL database (stores data in JSON-like documents).\
- **Express.js** → Lightweight backend web framework for Node.js.\
- **React.js** → Frontend library for building dynamic UIs.\
- **Node.js** → Runtime environment to run JavaScript on the server.

**Why popular?** - Full JavaScript stack (same language frontend &
backend).\
- JSON everywhere (easy data exchange).\
- High performance (non-blocking Node.js).\
- Large ecosystem + open source.

------------------------------------------------------------------------

### **Q2. Describe the typical architecture and data flow in a MERN application.**

**Answer:**\
1. **Client (React)** → Sends HTTP request (e.g., fetch API, Axios).\
2. **Server (Express + Node)** → Handles routes, processes logic.\
3. **Database (MongoDB)** → Stores/retrieves JSON data via Mongoose.\
4. **Response** → Server sends JSON back → React updates UI.

**Flow Example:**\
User submits form → React → POST `/api/users` → Express → MongoDB →
Response → React updates UI.

------------------------------------------------------------------------

### **Q3. How do the different components of the MERN stack interact?**

-   **React (frontend)** calls APIs (Axios/fetch).\
-   **Express (backend)** defines routes and controllers.\
-   **Node.js** executes backend logic, connects to MongoDB.\
-   **MongoDB** stores application data.

------------------------------------------------------------------------

### **Q4. How do you handle authentication & authorization in MERN?**

**Answer:**\
Using **JWT (JSON Web Token)**:\
1. User logs in → backend verifies credentials.\
2. Generate JWT with `jsonwebtoken`.\
3. Send JWT to client → stored in localStorage/cookies.\
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
response**.\
Uses: - Logging\
- Auth check\
- Parsing JSON\
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

**Answer:**\
- **Frontend (React)** → Vercel, Netlify, S3, or served by Express.\
- **Backend (Express + Node)** → Render, Heroku, AWS EC2, or Docker.\
- **Database (MongoDB)** → MongoDB Atlas (cloud).\
- Use **reverse proxy (NGINX/PM2)** for production.

------------------------------------------------------------------------

### **Q7. Benefits of MERN over other stacks?**

-   Single language (JS) → Faster development.\
-   JSON end-to-end.\
-   Large ecosystem + community.\
-   Scalable with microservices.

------------------------------------------------------------------------

## **MongoDB Questions**

### **Q1. What is MongoDB & how is it different from RDBMS?**

-   **MongoDB** = NoSQL, document-oriented, schema-less.\
-   **RDBMS** = Tables, fixed schema, joins.\
-   MongoDB stores flexible **JSON-like docs** → faster for modern apps.

------------------------------------------------------------------------

### **Q2. Key Features of MongoDB**

-   Document-oriented (BSON).\
-   Flexible schema.\
-   Horizontal scaling (sharding).\
-   Replication for high availability.\
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

-   **Replication** → Copies data across servers (high availability).\
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
function logger(req, res, next) {
  console.log("Request received");
  next();
}
app.use(logger);
```

------------------------------------------------------------------------

### **Q4. Error Handling**

``` js
app.use((err, req, res, next) => {
  res.status(500).send({ error: err.message });
});
```

------------------------------------------------------------------------

### **Q5. CORS**

``` js
const cors = require("cors");
app.use(cors({ origin: "http://localhost:3000" }));
```

------------------------------------------------------------------------

## **React.js Questions**

### **Q1. What is React.js?**

A UI library for building fast, reusable component-based apps.

Core Features → Components, Virtual DOM, JSX, Hooks.

------------------------------------------------------------------------

### **Q2. Virtual DOM**

-   In-memory representation of real DOM.\
-   React updates only changed parts → better performance.

------------------------------------------------------------------------

### **Q3. Components**

-   **Functional** (with hooks).\
-   **Class-based** (with lifecycle).

``` js
function Hello({ name }) {
  return <h1>Hello {name}</h1>;
}
```

------------------------------------------------------------------------

### **Q4. State Management**

-   Local → `useState`, `useReducer`.\
-   Global → Context API, Redux.

------------------------------------------------------------------------

### **Q5. Hooks Example**

``` js
const [count, setCount] = useState(0);
useEffect(() => console.log("Count changed"), [count]);
```

------------------------------------------------------------------------

### **Q6. Optimize React Performance**

-   Memoization (`React.memo`, `useMemo`).\
-   Code splitting (lazy load).\
-   Avoid unnecessary re-renders.

------------------------------------------------------------------------

### **Q7. Higher-Order Components (HOCs)**

A function that takes a component & returns a new one.\
Used for cross-cutting concerns (e.g., auth, logging).

------------------------------------------------------------------------

## **Node.js Questions**

### **Q1. What is Node.js?**

JS runtime built on Chrome V8 → runs JS on server.

------------------------------------------------------------------------

### **Q2. Event Loop**

Handles async tasks (I/O, timers) → non-blocking concurrency.

------------------------------------------------------------------------

### **Q3. Modules**

-   CommonJS (`require`)\
-   ES Modules (`import`).\
    Managed via **NPM**.

------------------------------------------------------------------------

### **Q4. Blocking vs Non-Blocking**

-   **Blocking** → Code waits (e.g., `fs.readFileSync`).\
-   **Non-blocking** → Uses callbacks/promises (`fs.readFile`).

------------------------------------------------------------------------

### **Q5. Streams & Buffers**

-   **Stream** → Handles large data in chunks (video, file).\
-   **Buffer** → Temporary memory storage for binary data.

------------------------------------------------------------------------

## **Scenario-Based & Advanced**

### **Q1. Real-time Data**

Use **WebSockets (Socket.IO)** for chat, notifications.

------------------------------------------------------------------------

### **Q2. Scaling**

-   Horizontal scaling with load balancers.\
-   Caching (Redis).\
-   Database sharding.

------------------------------------------------------------------------

### **Q3. Securing MERN App**

-   Use HTTPS.\
-   Store passwords with bcrypt.\
-   Use JWT securely (short expiry, refresh tokens).\
-   Sanitize inputs (prevent SQL/NoSQL injection, XSS).

------------------------------------------------------------------------

### **Q4. File Uploads**

Use **Multer (Express middleware)** → store in server or AWS S3.

------------------------------------------------------------------------

### **Q5. Testing Approaches**

-   **Unit** → Jest, Mocha.\
-   **Integration** → Supertest (API testing).\
-   **E2E** → Cypress, Playwright.

------------------------------------------------------------------------

✅ This covers **MERN + MongoDB + Express + React + Node + Advanced
Scenarios** with examples.
