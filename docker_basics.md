**Lab Title: Docker Container Basics**

**Objective:** Learn how to create and manage Docker containers.

**Prerequisites:** You should have Docker installed on your system. You can download and install Docker from the [official Docker website](https://www.docker.com/get-started).

**Lab Steps:**

**Step 1: Verify Docker Installation**
1. Open a terminal or command prompt.
2. Run the following command to verify that Docker is installed and working correctly:
   ```bash
   docker --version
   ```

**Step 2: Pull a Docker Image**
1. Pull a simple Docker image (e.g., the "hello-world" image) to check if your Docker setup is functioning:
   ```bash
   docker pull hello-world
   ```

2. Verify that the image has been downloaded by running:
   ```bash
   docker images
   ```

**Step 3: Run a Docker Container**
1. Run a container using the "hello-world" image:
   ```bash
   docker run hello-world
   ```

   This command will start a container based on the "hello-world" image, which will display a welcome message and then exit.

**Step 4: Create Your Own Docker Image**
1. Create a directory for your Docker project and navigate to it in the terminal.

2. Inside the directory, create a simple web application by creating an "index.html" file with some content.

3. Create a "Dockerfile" in the same directory with the following content:

   ```Dockerfile
   # Use an official Nginx runtime as the base image
   FROM nginx:latest

   # Copy your custom index.html to the Nginx web server's document root
   COPY index.html /usr/share/nginx/html
   ```

4. Build a Docker image from the Dockerfile:
   ```bash
   docker build -t my-webapp .
   ```

   This command tells Docker to build an image named "my-webapp" based on the instructions in your Dockerfile.

5. Verify that the new image has been created by running:
   ```bash
   docker images
   ```

**Step 5: Run a Container from Your Custom Image**
1. Run a container using the image you just created:
   ```bash
   docker run -d -p 8080:80 my-webapp
   ```

   This command runs a container based on your "my-webapp" image, and it maps port 8080 on your host to port 80 inside the container.

2. Access the web application in your web browser by going to `http://localhost:8080`. You should see the content from your custom "index.html" file.

**Step 6: Manage Containers**
1. List running containers:
   ```bash
   docker ps
   ```

2. Stop and remove the container you just created:
   ```bash
   docker stop <container_id>
   docker rm <container_id>
   ```

   Replace `<container_id>` with the actual container ID from the previous `docker ps` command.
