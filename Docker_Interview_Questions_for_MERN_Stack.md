# 🐳 Docker Interview Questions for MERN Stack Developers

## 🔹 1. What is Docker?

Docker is a **containerization platform** that allows you to **package
an application and its dependencies** into containers, ensuring it runs
consistently across different environments.

------------------------------------------------------------------------

## 🔹 2. Why use Docker in a MERN stack project?

-   Ensures consistent environments across development, testing, and
    production.\
-   Simplifies deployment.\
-   Makes it easy to isolate services (MongoDB, Node backend, React
    frontend).\
-   Helps scale microservices independently.

------------------------------------------------------------------------

## 🔹 3. What is a Docker container?

A container is a **lightweight, isolated environment** that runs an
application and all its dependencies using a Docker image.

------------------------------------------------------------------------

## 🔹 4. What is a Docker image?

A Docker image is a **read-only template** that defines how a container
should be built --- it includes the application code, OS libraries, and
configurations.

------------------------------------------------------------------------

## 🔹 5. What is a Dockerfile?

A Dockerfile is a **script containing a series of instructions** used to
build a Docker image.

**Example for Node.js (Express) app:**

``` dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

------------------------------------------------------------------------

## 🔹 6. How would you Dockerize a MERN application?

1.  Create separate Dockerfiles for **client (React)**, **server
    (Node/Express)**, and **MongoDB**.\
2.  Use **Docker Compose** to link all services together.

------------------------------------------------------------------------

## 🔹 7. What is Docker Compose and why is it useful?

Docker Compose allows you to **define and manage multiple containers**
(e.g., MongoDB, backend, frontend) using a single `docker-compose.yml`
file.

**Example:**

``` yaml
version: '3'
services:
  mongo:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"

  backend:
    build: ./server
    ports:
      - "5000:5000"
    depends_on:
      - mongo

  frontend:
    build: ./client
    ports:
      - "3000:3000"
```

------------------------------------------------------------------------

## 🔹 8. How do you connect your Node.js app with MongoDB inside Docker?

Use the **service name (not localhost)** from Docker Compose as the
host.

``` js
mongoose.connect("mongodb://mongo:27017/mydb");
```

------------------------------------------------------------------------

## 🔹 9. What is the difference between `COPY` and `ADD` in Dockerfile?

  -----------------------------------------------------------------------
  Command                       Description
  ----------------------------- -----------------------------------------
  `COPY`                        Copies files from host to container
                                (simple, preferred)

  `ADD`                         Similar to COPY but can also extract
                                `.tar` files and download URLs
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 🔹 10. What is the difference between a container and a virtual machine?

  Feature     Docker Container        Virtual Machine
  ----------- ----------------------- -------------------
  OS          Shares host OS kernel   Has its own OS
  Size        Lightweight (MBs)       Heavy (GBs)
  Speed       Starts in seconds       Starts in minutes
  Isolation   Process-level           Hardware-level

------------------------------------------------------------------------

## 🔹 11. How do you persist MongoDB data in Docker?

Use **Docker volumes**:

``` yaml
volumes:
  mongo-data:
services:
  mongo:
    image: mongo
    volumes:
      - mongo-data:/data/db
```

------------------------------------------------------------------------

## 🔹 12. How do you check running containers and logs?

-   List running containers:

    ``` bash
    docker ps
    ```

-   View logs:

    ``` bash
    docker logs <container_name>
    ```

------------------------------------------------------------------------

## 🔹 13. What is the `.dockerignore` file?

It's similar to `.gitignore` --- it tells Docker **which files or
folders to exclude** when building an image.

**Example:**

    node_modules
    .git
    .env

------------------------------------------------------------------------

## 🔹 14. What is multi-stage build in Docker?

It's a way to **optimize image size** by using multiple `FROM`
statements --- one for building the app, another for serving only the
final output.

**Example (React app):**

``` dockerfile
# Build stage
FROM node:18 AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
```

------------------------------------------------------------------------

## 🔹 15. How do you deploy a Dockerized MERN app?

-   Push the Docker images to **Docker Hub** or **ECR**.\
-   Use **Docker Compose** or **Kubernetes** to run them on a server
    (AWS, GCP, etc).\
-   Configure environment variables using `.env` files.

------------------------------------------------------------------------

✅ **Tip:** Practice building, running, and linking containers ---
that's often tested in live interviews!
