### Exercise: Docker Logging Configuration

**Objective:** Practice configuring Docker container logging using different logging drivers and options.

**Prerequisites:**
- Docker installed on your system.

**Duration:** Approximately 30-45 minutes.

**Step 1: Verify Docker Installation**

Ensure that Docker is properly installed on your system by running:

```bash
docker --version
```

**Step 2: Explore Available Logging Drivers**

List the available logging drivers that Docker supports using:

```bash
docker info --format '{{.LoggingDriver}}'
```

This command will display the default logging driver, which is typically "json-file."

**Step 3: Configure Logging to Stdout**

Create a Docker container running an Nginx web server with logs sent to the standard output (stdout):

```bash
docker run -d --name nginx-container -p 8080:80 nginx
```

By default, the logs from this container will be sent to stdout.

**Step 4: View Container Logs**

View the container logs using the `docker logs` command:

```bash
docker logs nginx-container
```

You should see the Nginx access logs.

**Step 5: Configure Logging to Syslog**

Stop and remove the Nginx container:

```bash
docker stop nginx-container
docker rm nginx-container
```

Now, create a new Docker container with the logging driver set to "syslog":

```bash
docker run -d --name syslog-nginx-container --log-driver=syslog -p 8080:80 nginx
```

**Step 6: View Syslog Output (Linux)**

On Linux systems, you can view the syslog output to see the container logs. Open a terminal and run:

```bash
sudo tail -f /var/log/syslog
```

You should see log entries related to the "syslog-nginx-container."

**Step 7: Configure Logging to JSON File**

Stop and remove the syslog-nginx-container:

```bash
docker stop syslog-nginx-container
docker rm syslog-nginx-container
```

Create a new Docker container with the logging driver set to "json-file":

```bash
docker run -d --name json-file-nginx-container --log-driver=json-file -p 8080:80 nginx
```

**Step 8: View JSON File Logs**

Inspect the JSON file logs by locating the log file for the container:

```bash
docker inspect -f '{{.LogPath}}' json-file-nginx-container
```

Use a text editor to open and view the contents of the log file.

**Step 9: Cleanup**

Remove the containers created during the exercise:

```bash
docker stop json-file-nginx-container
docker rm json-file-nginx-container
```

**Conclusion:**

This exercise helped you practice configuring Docker container logging using different logging drivers and options. Docker provides flexibility in logging, allowing you to choose the best logging driver for your use case and send logs to various destinations such as stdout, syslog, or log files. Understanding Docker's logging capabilities is essential for monitoring and troubleshooting containerized applications.
