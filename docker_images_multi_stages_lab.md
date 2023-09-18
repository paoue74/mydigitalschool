**Lab Title: Building Docker Images with Multi-Stage Builds**

**Objective:** Learn how to create a Docker image for a Node.js application using multi-stage builds to optimize image size and improve security.

**Prerequisites:** You should have Docker installed on your system.

**Lab Steps:**

**Step 1: Create a Node.js Application**
1. Open a terminal or command prompt.

2. Create a directory for your Node.js application project and navigate to it:

   ```bash
   mkdir my-node-app
   cd my-node-app
   ```

3. Inside the project directory, create a simple Node.js application. For example, create a file named `app.js` with the following content:

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
       res.writeHead(200, { 'Content-Type': 'text/plain' });
       res.end('Hello, Docker Multi-Stage Build!\n');
   });

   const port = process.env.PORT || 3000;
   server.listen(port, () => {
       console.log(`Server is listening on port ${port}`);
   });
   ```

**Step 2: Create a Multi-Stage Dockerfile**
1. Inside the project directory, create a file named `Dockerfile` (without any file extension) with the following content:

   ```Dockerfile
   # Stage 1: Build the Node.js application
   FROM node:14 AS build
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   RUN npm run build

   # Stage 2: Create the production image
   FROM node:14 AS production
   WORKDIR /app
   COPY --from=build /app/package*.json ./
   RUN npm install --only=production
   COPY --from=build /app/dist ./dist
   EXPOSE 3000
   CMD ["node", "dist/app.js"]
   ```

   This multi-stage Dockerfile defines two stages:
   - **Stage 1 (build)**: Builds the Node.js application, including installing development dependencies and running any build scripts.
   - **Stage 2 (production)**: Creates the production image with only production dependencies and the built application. This stage is smaller and more secure.

**Step 3: Build the Docker Image**
1. Build a Docker image from the multi-stage Dockerfile in your project directory:

   ```bash
   docker build -t my-node-app .
   ```

   This command tells Docker to build an image named "my-node-app" based on the instructions in your multi-stage Dockerfile.

2. Verify that the new image has been created by running:

   ```bash
   docker images
   ```

**Step 4: Run a Container from the Custom Image**
1. Run a container from the custom image you just built:

   ```bash
   docker run -p 3000:3000 my-node-app
   ```

   You should see the message "Server is listening on port 3000" in the terminal, indicating that the Node.js application is running inside the container.

2. Access the Node.js application by going to `http://localhost:3000`. You should see the message "Hello, Docker Multi-Stage Build!" displayed in your browser.

**Step 5: Clean Up**
1. Stop the running container by pressing `Ctrl+C` in the terminal where it's running.

2. Remove the container:

   ```bash
   docker rm <container_id>
   ```

   Replace `<container_id>` with the actual container ID from the previous `docker ps -a` command.
