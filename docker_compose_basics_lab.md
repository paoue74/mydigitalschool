Certainly! Here's another Docker lab exercise that goes a bit further and introduces the use of Docker Compose for managing multi-container applications.

**Lab Title: Docker Compose Basics**

**Objective:** Learn how to use Docker Compose to manage multi-container applications.

**Prerequisites:** You should have Docker and Docker Compose installed on your system. Docker Compose is typically included with Docker Desktop on Windows and macOS or can be installed separately on Linux. You can install it following the instructions in the [official Docker Compose documentation](https://docs.docker.com/compose/install/).

**Lab Steps:**

**Step 1: Create a Docker Compose Project**
1. Create a new directory for your Docker Compose project and navigate to it in the terminal.

**Step 2: Define a Docker Compose File**
1. Inside the project directory, create a file named `docker-compose.yml` with the following content:

   ```yaml
   version: '3'
   services:
     web:
       image: nginx:latest
       ports:
         - "8080:80"
     app:
       image: python:3.9
       command: python -m http.server 8000
       ports:
         - "8000:8000"
   ```

   This Docker Compose file defines two services: "web" (an Nginx web server) and "app" (a Python web server).

**Step 3: Start the Docker Compose Project**
1. Run the following command in your project directory to start the services defined in the `docker-compose.yml` file:

   ```bash
   docker-compose up
   ```

   Docker Compose will download the necessary images (if not already downloaded) and start the containers.

2. Access the Nginx web server in your web browser by going to `http://localhost:8080`. You should see the default Nginx welcome page.

3. Access the Python web server by going to `http://localhost:8000`. You should see a simple Python HTTP server response.

**Step 4: Manage Docker Compose Services**
1. In the terminal where Docker Compose is running, press `Ctrl+C` to stop the services.

2. To start the services in the background (detached mode), use the following command:

   ```bash
   docker-compose up -d
   ```

3. To stop and remove the services, run:

   ```bash
   docker-compose down
   ```

**Step 5: Scale the "app" Service**
1. Modify the `docker-compose.yml` file to include a new "app" service definition with a different command:

   ```yaml
   version: '3'
   services:
     web:
       image: nginx:latest
       ports:
         - "8080:80"
     app:
       image: python:3.9
       command: python -m http.server 8000
       ports:
         - "8000:8000"
     app2:
       image: python:3.9
       command: python -m http.server 8000
       ports:
         - "8001:8000"
   ```

2. Run the following command to scale the "app" service to two instances:

   ```bash
   docker-compose up -d --scale app=2
   ```

3. Access the second Python web server by going to `http://localhost:8001`.

**Lab Title: Multi-Container Application with Docker Compose**

**Objective:** Learn how to use Docker Compose to define and manage a multi-container application.

**Prerequisites:** You should have Docker and Docker Compose installed on your system.

**Lab Steps:**

**Step 1: Create a Docker Compose File**
1. Open a terminal or command prompt.

2. Create a new directory for your Docker Compose project and navigate to it:

   ```bash
   mkdir my-docker-compose-app
   cd my-docker-compose-app
   ```

3. Inside the project directory, create a file named `docker-compose.yml` with the following content:

   ```yaml
   version: '3'
   services:
     web:
       image: nginx:latest
       ports:
         - "8080:80"
     app:
       image: node:14
       working_dir: /app
       volumes:
         - ./app:/app
       command: ["npm", "start"]
   ```

   This Docker Compose file defines two services: "web" (an Nginx web server) and "app" (a Node.js application).

**Step 2: Create a Node.js Application**
1. Create a directory named `app` inside your project directory:

   ```bash
   mkdir app
   ```

2. Inside the `app` directory, create a simple Node.js application with a `package.json` and a sample JavaScript file. For example:

   - Create a `package.json` file with the following content:

     ```json
     {
       "name": "my-docker-app",
       "version": "1.0.0",
       "scripts": {
         "start": "node server.js"
       },
       "dependencies": {}
     }
     ```

   - Create a `server.js` file with a simple Node.js web server:

     ```javascript
     const http = require('http');

     const server = http.createServer((req, res) => {
       res.writeHead(200, { 'Content-Type': 'text/plain' });
       res.end('Hello Docker Compose!\n');
     });

     const port = process.env.PORT || 3000;
     server.listen(port, () => {
       console.log(`Server is listening on port ${port}`);
     });
     ```

**Step 3: Start the Docker Compose Application**
1. From your project directory, run the following command to start the services defined in the `docker-compose.yml` file:

   ```bash
   docker-compose up
   ```

   Docker Compose will download the necessary images (if not already downloaded) and start the containers.

2. Access the Nginx web server in your web browser by going to `http://localhost:8080`. You should see the default Nginx welcome page.

3. Access the Node.js application by going to `http://localhost:3000`. You should see the message "Hello Docker Compose!" displayed in your browser.

**Step 4: Stop and Clean Up**
1. In the terminal where Docker Compose is running, press `Ctrl+C` to stop the services.

2. To remove the stopped containers and network, run:

   ```bash
   docker-compose down
   ```
