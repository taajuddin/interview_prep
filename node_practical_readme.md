# Node.js Practical Problems & Solutions

## 1. Socket.IO -- Real‑World Problem: Real‑Time Chat System

### Problem

Build a chat system where users send/receive live messages.

### Example

``` js
const io = require("socket.io")(3000);

io.on("connection", (socket) => {
  console.log("User connected:", socket.id);

  socket.on("chat:message", (msg) => {
    io.emit("chat:message", { id: socket.id, msg });
  });

  socket.on("disconnect", () => console.log("User left"));
});
```

------------------------------------------------------------------------

## 2. EventEmitter -- Real‑World Problem: Login Event Logger

### Problem

Track user login events and write logs.

### Example

``` js
const EventEmitter = require("events");
class LoginEmitter extends EventEmitter {}
const logger = new LoginEmitter();

logger.on("logged_in", (data) => {
  console.log(`User logged in: ${data.user}`);
});

logger.emit("logged_in", { user: "john_doe" });
```

------------------------------------------------------------------------

## 3. Clustering -- Real‑World Problem: Scale CPU‑heavy API

### Problem

Increase throughput of Node.js server using all CPU cores.

### Example

``` js
const cluster = require("cluster");
const http = require("http");
const os = require("os");

if (cluster.isMaster) {
  os.cpus().forEach(() => cluster.fork());

  cluster.on("exit", () => cluster.fork());
} else {
  http.createServer((req, res) => {
    res.end(`Handled by worker ${process.pid}`);
  }).listen(3000);
}
```

------------------------------------------------------------------------

## 4. Streams -- Real‑World Problem: Read Large File Without Loading Into Memory

### Example

``` js
const fs = require("fs");

const stream = fs.createReadStream("largefile.txt", "utf8");
stream.on("data", (chunk) => console.log("Chunk:", chunk.length));
stream.on("end", () => console.log("Done"));
```

------------------------------------------------------------------------

## 5. FS (File System) -- Real‑World Problem: Append Logs to File

### Example

``` js
const fs = require("fs");

fs.appendFile("app.log", "Server restarted\n", (err) => {
  if (err) throw err;
  console.log("Log saved");
});
```

------------------------------------------------------------------------

## 6. AWS S3 -- Real‑World Problem: Upload File to Bucket

### Example

``` js
const AWS = require("aws-sdk");
const fs = require("fs");

AWS.config.update({ region: "ap-south-1" });
const s3 = new AWS.S3();

const upload = () => {
  const fileStream = fs.createReadStream("image.png");
  const params = {
    Bucket: "my-bucket",
    Key: "uploads/image.png",
    Body: fileStream,
  };

  return s3.upload(params).promise();
};

upload().then(console.log).catch(console.error);
```

------------------------------------------------------------------------

## 7. AWS Lambda -- Real‑World Problem: Serverless API Handler

### Example

``` js
exports.handler = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ message: "Hello from Lambda!" }),
  };
};
```

------------------------------------------------------------------------

## 8. AWS EC2 -- Real‑World Problem: Running Node.js App on EC2

### Basic Startup Script

``` bash
#!/bin/bash
sudo yum update -y
curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -
sudo yum install -y nodejs git
git clone https://github.com/my/app.git
cd app
npm install
node server.js
```

------------------------------------------------------------------------

## END
