### Lab: Using Docker Secrets

**Objective:** Create and use Docker secrets to securely manage sensitive information within Docker containers.

**Prerequisites:**
- Docker installed on your system.

**Duration:** Approximately 30-45 minutes.

**Step 1: Create a Docker Swarm**

Docker secrets are primarily designed for use in Docker Swarm mode. Initialize a Docker Swarm on your local machine if you haven't already:

```bash
docker swarm init
```

**Step 2: Create a Docker Secret**

Create a Docker secret containing a sample password:

```bash
echo "MySecretPassword" | docker secret create my_secret_password -
```

Verify that the secret has been created:

```bash
docker secret ls
```

**Step 3: Create a Docker Service with a Secret**

Now, create a Docker service that uses the secret you've just created. In this example, we'll create a simple Nginx web server service that serves a webpage with the secret:

```bash
docker service create --name nginx-secret --secret my_secret_password -p 80:80 nginx:alpine
```

**Step 4: Create an HTML File with Secret**

Create a simple HTML file (`index.html`) that displays the secret. You'll mount this file into the Nginx container to demonstrate the use of the secret.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Secret Example</title>
</head>
<body>
    <h1>My Secret Password:</h1>
    <?php
        $secret = file_get_contents('/run/secrets/my_secret_password');
        echo "<p>$secret</p>";
    ?>
</body>
</html>
```

**Step 5: Create a Docker Config for the HTML File**

Docker secrets are typically used to manage binary data. However, to demonstrate another feature, let's create a Docker config for the HTML file:

```bash
docker config create my_html_config index.html
```

**Step 6: Deploy the Service with Secrets and Configs**

Update the Nginx service to use the Docker secret (`my_secret_password`) and Docker config (`my_html_config`) by mounting them into the container:

```bash
docker service update --secret-rm my_secret_password --config-rm my_html_config nginx-secret
docker service update --secret-add source=my_secret_password,target=my_secret_password nginx-secret
docker service update --config-add source=my_html_config,target=/usr/share/nginx/html/index.html nginx-secret
```

**Step 7: Access the Service**

After the service has been updated, access the Nginx service in your web browser:

- Open a web browser and navigate to http://localhost:80.

You should see a webpage displaying the secret password.

**Step 8: Cleanup**

Clean up the resources created during this lab:

```bash
docker service rm nginx-secret
docker secret rm my_secret_password
docker config rm my_html_config
```

**Conclusion:**

This lab exercise demonstrated how to use Docker secrets to securely manage sensitive information within Docker containers. Docker secrets are a powerful tool for managing secrets in a Docker Swarm environment and help enhance the security of your containerized applications.
