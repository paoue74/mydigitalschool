### Exercise: Docker Resource Limitation and Monitoring

**Objective:** In this exercise, you will learn how to limit CPU and memory resources for Docker containers, monitor resource usage, and observe the effects of resource limitations.

**Prerequisites:** 
- Docker installed on your system.
- Basic knowledge of Docker and Docker Compose.

**Duration:** Approximately 45-60 minutes.

**Step 1: Create a Sample Application**
- Create a directory for your sample application, e.g., `resource-limit-demo`.
- Inside the directory, create a simple Python script (`app.py`) that consumes CPU and memory resources. For example:

   ```python
   import os
   import time

   # Infinite loop to consume CPU
   while True:
       pass
   ```

- Create a Dockerfile to containerize your application:

   ```Dockerfile
   # Use an official Python runtime as a base image
   FROM python:3

   # Set a working directory
   WORKDIR /app

   # Copy your application code into the container
   COPY app.py .

   # Run your application
   CMD [ "python", "app.py" ]
   ```

**Step 2: Build and Run a Container Without Resource Limits**
- Build the Docker image for your application and run a container without specifying resource limits:

   ```bash
   docker build -t resource-demo .
   docker run -d resource-demo
   ```

- Use `docker stats` to monitor resource usage:

   ```bash
   docker stats
   ```

- Observe how the container consumes CPU and memory without any limitations.

**Step 3: Limit CPU and Memory Resources**
- Stop the running container.

   ```bash
   docker stop <container_id>
   ```

- Modify your Docker Compose file (`docker-compose.yml`) to specify CPU and memory limits for your container:

   ```yaml
   version: '3'

   services:
     app:
       image: resource-demo
       deploy:
         resources:
           limits:
             cpus: '0.5'  # Limit to half a CPU core
             memory: 256M  # Limit to 256MB of memory
   ```

- Deploy your application with Docker Compose:

   ```bash
   docker-compose up -d
   ```

**Step 4: Monitor Resource Usage with Resource Limits**
- Use `docker stats` to monitor the resource usage of the container with resource limits.

   ```bash
   docker stats
   ```

- Observe how the container's CPU usage is limited to 50% (0.5 CPU core) and memory usage is limited to 256MB.

**Step 5: Exceed Resource Limits (Optional)**
- If you'd like, you can modify the CPU limit in the Docker Compose file to be lower than the workload requires and recreate the container. Observe how Docker enforces resource limits and throttles the container when limits are exceeded.

**Step 6: Cleanup**
- Stop and remove the containers and resources created during the exercise.

   ```bash
   docker-compose down
   ```

**Conclusion:** 
- Resource limits are crucial for preventing resource contention and ensuring predictable container behavior.
- By monitoring resource usage and setting appropriate limits, you can manage container resource allocation effectively.
