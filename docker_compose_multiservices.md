Here's the directory structure for our multi-service application:

```plaintext
my-multi-service-app/
  ├── frontend/
  │   ├── Dockerfile
  │   ├── package.json
  │   ├── package-lock.json
  │   ├── public/
  │   └── src/
  ├── backend/
  │   ├── Dockerfile
  │   ├── package.json
  │   ├── package-lock.json
  │   └── app.js
  ├── docker-compose.yml
  └── .env
```

Now, let's create the necessary files and configurations for each part of our multi-service application.

#### `frontend/Dockerfile` (React Frontend):

```Dockerfile
# Use an official Node.js runtime as a base image
FROM node:14 AS builder

WORKDIR /app/frontend

# Copy and install frontend dependencies
COPY frontend/package*.json ./
RUN npm install

# Copy frontend source code
COPY frontend/ .

# Build the frontend
RUN npm run build

# Use Nginx as a lightweight server to serve the frontend
FROM nginx:alpine

# Copy built frontend files from the builder stage to Nginx
COPY --from=builder /app/frontend/build /usr/share/nginx/html

# Expose port 80 for Nginx
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

#### `backend/Dockerfile` (Node.js Backend):

```Dockerfile
# Use an official Node.js runtime as a base image
FROM node:14

WORKDIR /app/backend

# Copy and install backend dependencies
COPY backend/package*.json ./
RUN npm install

# Copy backend source code
COPY backend/ .

# Expose port 3000 for the backend service
EXPOSE 3000

# Start the backend service
CMD ["node", "app.js"]
```

#### `docker-compose.yml` (Docker Compose Configuration):

```yaml
version: '3'
services:
  frontend:
    build:
      context: .
      dockerfile: frontend/Dockerfile
    ports:
      - "80:80"
  backend:
    build:
      context: .
      dockerfile: backend/Dockerfile
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydb
```

#### `.env` (Environment Variables for Docker Compose):

```plaintext
POSTGRES_HOST=db
POSTGRES_PORT=5432
```

In this example:

- The `frontend` service uses the React frontend Dockerfile to build the frontend and serves it using Nginx on port 80.
- The `backend` service uses the Node.js backend Dockerfile to run the backend on port 3000. It depends on the `db` service.
- The `db` service uses the official PostgreSQL image and sets environment variables for the PostgreSQL database.

To deploy this multi-service application with Docker Compose, follow these steps:

1. Create the directory structure and files as described above.

2. Make sure Docker and Docker Compose are installed on your system.

3. Navigate to the root directory (`my-multi-service-app`) in your terminal.

4. Build and start the application containers with Docker Compose:

   ```bash
   docker-compose up --build
   ```

   This command will build the Docker images for both frontend and backend and start all services defined in `docker-compose.yml`.

5. Access the frontend application in your web browser at `http://localhost`.

6. You can also access the backend API at `http://localhost:3000`. The backend communicates with the PostgreSQL database defined in the `db` service.

7. To stop the containers, press `Ctrl+C` in the terminal where `docker-compose` is running.
