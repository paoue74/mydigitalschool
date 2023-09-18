**Lab Title: Docker Custom Bridge Network**

**Objective:** Learn how to create a custom bridge network for Docker containers and observe network communication between containers.

**Prerequisites:** You should have Docker installed on your system.

**Lab Steps:**

**Step 1: Create a Custom Bridge Network**
1. Open a terminal or command prompt.

2. Create a custom Docker bridge network named "my-network" using the following command:

   ```bash
   docker network create my-network
   ```

**Step 2: Run Containers on the Custom Bridge Network**
1. Run two Docker containers on the "my-network" custom bridge network. In this example, we'll use the "alpine" image:

   ```bash
   docker run -d --network my-network --name container1 alpine sleep 3600
   docker run -d --network my-network --name container2 alpine sleep 3600
   ```

   - `-d`: Run the containers in detached mode.
   - `--network my-network`: Attach the containers to the "my-network" custom bridge network.
   - `--name container1` and `--name container2`: Assign names to the containers for easy identification.
   - `alpine sleep 3600`: Use the "alpine" image with a sleep command to keep the containers running.

**Step 3: Inspect Network Configuration**
1. Inspect the "my-network" custom bridge network to view its details:

   ```bash
   docker network inspect my-network
   ```

   This command will display information about the network, including container endpoints and IP addresses.

**Step 4: Test Network Connectivity**
1. Access one of the containers by running a shell inside it. For example, access "container1":

   ```bash
   docker exec -it container1 sh
   ```

2. From within "container1," try to ping "container2" by its container name:

   ```bash
   ping container2
   ```

   You should see successful network communication between the containers.

3. Exit the shell inside "container1."

**Step 5: Clean Up**
1. Stop and remove the containers:

   ```bash
   docker stop container1 container2
   docker rm container1 container2
   ```

2. Remove the custom bridge network:

   ```bash
   docker network rm my-network
   ```
