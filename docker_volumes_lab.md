**Lab Title: Docker Volumes and Data Persistence**

**Objective:** Learn how to use Docker volumes for data persistence and share data between containers.

**Prerequisites:** You should have Docker installed on your system.

**Lab Steps:**

**Step 1: Create a Docker Volume**
1. Open a terminal or command prompt.

2. Create a Docker volume named "mydata" using the following command:

   ```bash
   docker volume create mydata
   ```

**Step 2: Run a Container with a Volume**
1. Run a Docker container with a volume mounted to it. In this example, we'll use an official Nginx image:

   ```bash
   docker run -d -p 8080:80 -v mydata:/usr/share/nginx/html --name my-nginx nginx:latest
   ```

   - `-d`: Run the container in detached mode.
   - `-p 8080:80`: Map port 8080 on your host to port 80 inside the container.
   - `-v mydata:/usr/share/nginx/html`: Mount the "mydata" volume to the Nginx container's document root.
   - `--name my-nginx`: Assign a name to the container.

2. Access the Nginx web server in your web browser by going to `http://localhost:8080`. You should see the default Nginx welcome page.

**Step 3: Inspect the Volume**
1. Inspect the "mydata" volume to view its details:

   ```bash
   docker volume inspect mydata
   ```

   This command will display information about the volume, including its location on the host system.

**Step 4: Create and Share Data with a Second Container**
1. Create a simple text file on your host machine that you want to share with another container. For example, create a file named "mydata.txt" with some content.

2. Run a second container and mount the same "mydata" volume to it:

   ```bash
   docker run -it --rm -v mydata:/data alpine:latest sh
   ```

   This command runs an Alpine Linux container, mounts the "mydata" volume to the "/data" directory inside the container, and opens an interactive shell.

3. Inside the Alpine container, navigate to the "/data" directory and list the contents. You should see the "mydata.txt" file.

**Step 5: Clean Up**
1. Stop and remove the Nginx container:

   ```bash
   docker stop my-nginx
   docker rm my-nginx
   ```

2. Remove the "mydata" volume:

   ```bash
   docker volume rm mydata
   ```
