**Lab Title: Building a Custom Docker Image for a Java Application**

**Objective:** Learn how to create a custom Docker image for a Java application using Dockerfiles and run a container from that image.

**Prerequisites:** You should have Docker and a Java Development Kit (JDK) installed on your system.

**Lab Steps:**

**Step 1: Create a Java Application**
1. Open a terminal or command prompt.

2. Create a directory for your Java application project and navigate to it:

   ```bash
   mkdir my-java-app
   cd my-java-app
   ```

3. Inside the project directory, create a simple Java application. For example, create a file named `HelloWorld.java` with the following content:

   ```java
   public class HelloWorld {
       public static void main(String[] args) {
           System.out.println("Hello, Docker Java World!");
       }
   }
   ```

**Step 2: Create a Dockerfile**
1. Inside the project directory, create a file named `Dockerfile` (without any file extension) with the following content:

   ```Dockerfile
   # Use an official OpenJDK runtime as the base image
   FROM openjdk:11-slim

   # Set the working directory to /app
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY . /app

   # Compile the Java application
   RUN javac HelloWorld.java

   # Run the Java application when the container launches
   CMD ["java", "HelloWorld"]
   ```

   This Dockerfile specifies the base image as OpenJDK 11, sets the working directory to `/app`, copies the Java source code into the container, compiles it, and then runs the Java application.

**Step 3: Build the Custom Docker Image**
1. Build a Docker image from the Dockerfile in your project directory:

   ```bash
   docker build -t my-java-app .
   ```

   This command tells Docker to build an image named "my-java-app" based on the instructions in your Dockerfile.

2. Verify that the new image has been created by running:

   ```bash
   docker images
   ```

**Step 4: Run a Container from the Custom Image**
1. Run a container from the custom image you just built:

   ```bash
   docker run my-java-app
   ```

   You should see the output "Hello, Docker Java World!" in the terminal, indicating that the Java application is running inside the container.

**Step 5: Clean Up**
1. Stop the running container by pressing `Ctrl+C` in the terminal where it's running.

2. Remove the container:

   ```bash
   docker rm <container_id>
   ```

   Replace `<container_id>` with the actual container ID from the previous `docker ps -a` command.
