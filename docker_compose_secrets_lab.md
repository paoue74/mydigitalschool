### Lab: Using Docker Compose for Simulated Secrets

**Objective:** Simulate the usage of Docker secrets within Docker Compose to securely manage sensitive information for your containers.

**Prerequisites:**
- Docker and Docker Compose installed on your system.

**Duration:** Approximately 30-45 minutes.

**Step 1: Create a Docker Compose File**

Create a Docker Compose file (`docker-compose.yml`) to define your services. In this example, we'll use an Nginx web server and a simple PHP script to display the secret.

```yaml
version: '3'
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
  php:
    image: php:alpine
    volumes:
      - ./php:/var/www/html
    depends_on:
      - web
```

**Step 2: Create a Directory for Files**

Create two directories, `html` and `php`, in the same directory as your Docker Compose file. Place an HTML file (`index.html`) and a PHP script (`index.php`) inside these directories:

**`./html/index.html`**:

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

**`./php/index.php`**:

```php
<?php
$secret = file_get_contents('/run/secrets/my_secret_password');
echo "My Secret Password: $secret";
?>
```

**Step 3: Create a Docker Compose Override File**

Create a Docker Compose override file (`docker-compose.override.yml`) to define secrets for the services:

```yaml
version: '3'
services:
  web:
    secrets:
      - my_secret_password
  php:
    secrets:
      - my_secret_password
secrets:
  my_secret_password:
    file: ./secrets/my_secret_password.txt
```

**Step 4: Create a Directory for Secrets**

Create a directory called `secrets` in the same directory as your Docker Compose files. Inside this directory, create a text file (`my_secret_password.txt`) containing your secret:

**`./secrets/my_secret_password.txt`**:

```
MySecretPassword
```

**Step 5: Deploy the Services**

Deploy your services using Docker Compose:

```bash
docker-compose up -d
```

**Step 6: Access the Service**

After the services have been deployed, access the Nginx service in your web browser:

- Open a web browser and navigate to http://localhost:80.

You should see a webpage displaying the secret password.

**Step 7: Cleanup**

Clean up the resources created during this lab:

```bash
docker-compose down
```

**Conclusion:**

This lab exercise demonstrated how to simulate the usage of Docker secrets within Docker Compose by using a combination of volumes and Docker Compose override files. While Docker Compose does not have native support for secrets like Docker Swarm, you can use this approach to securely manage sensitive information for your containers in a development or testing environment. For production use cases, consider using Docker Swarm or another orchestration platform with built-in secrets management.
