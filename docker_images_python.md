**Lab Title: Building Custom Docker Images**

**Objective:** Learn how to create custom Docker images using Dockerfiles and run containers from those images.

**Prerequisites:** You should have Docker installed on your system.

**Lab Steps:**

**Step 1: Create a Dockerfile**
1. Open a terminal or command prompt.

2. Create a new directory for your Docker image project and navigate to it:

   ```bash
   mkdir my-docker-image
   cd my-docker-image
   ```

3. Inside the project directory, create a file named `Dockerfile` (without any file extension) with the following content:

   ```Dockerfile
   # Use an official Python runtime as the base image
   FROM python:3.9-slim-buster

   # Set the working directory to /app
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY . /app

   # Install any needed packages specified in requirements.txt
   RUN pip install --no-cache-dir -r requirements.txt

   # Make port 80 available to the world outside this container
   EXPOSE 80

   # Define environment variable
   ENV NAME World

   # Run app.py when the container launches
   CMD ["python", "app.py"]
   ```

**Step 2: Create a Python Application**
1. Inside the project directory, create a Python application by creating a file named `app.py` with the following content:

   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def hello():
       return "Hello, Docker World!"

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=80)
   ```

2. Create a file named `requirements.txt` to specify Python dependencies:

   ```
   Flask==2.0.1
   ```

**Step 3: Build the Custom Docker Image**
1. Build a Docker image from the Dockerfile in your project directory:

   ```bash
   docker build -t my-python-app .
   ```

   This command tells Docker to build an image named "my-python-app" based on the instructions in your Dockerfile.

2. Verify that the new image has been created by running:

   ```bash
   docker images
   ```

**Step 4: Run a Container from the Custom Image**
1. Run a container from the custom image you just built:

   ```bash
   docker run -p 4000:80 my-python-app
   ```

   - `-p 4000:80`: Map port 4000 on your host to port 80 inside the container.

2. Access the Python application in your web browser by going to `http://localhost:4000`. You should see the message "Hello, Docker World!" displayed in your browser.

**Step 5: Clean Up**
1. Stop the running container by pressing `Ctrl+C` in the terminal where it's running.

2. Remove the container:

   ```bash
   docker rm <container_id>
   ```

   Replace `<container_id>` with the actual container ID from the previous `docker ps -a` command.
