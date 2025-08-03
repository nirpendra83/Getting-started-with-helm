---
title: "Session 02"
description: "Learn how to use Docker Compose to define and run multi-container applications with ease."
date: 2025-08-02
draft: false
tags: ["docker", "docker-compose", "containers", "devops"]
---

## 📦 Docker Compose – Manage Multi-Container Applications

---

## 📌 What is Docker Compose?

**Docker Compose** is a tool used to define and run multi-container Docker applications.

You define the services, networks, and volumes your app needs in a **`docker-compose.yml`** file, and bring everything up with a single command.

---

## ✅ Benefits of Docker Compose

- Easy to manage complex applications  
- Reproducible environments  
- Centralized configuration  
- Supports custom networks and volumes  
- Ideal for local development and CI pipelines  

---

## [Docker Compose Installation](https://docs.docker.com/compose/)
## 🧱 Example Project Structure

```
myapp/
├── backend/
│   └── app.py
├── frontend/
│   └── index.html
├── db/
│   └── init.sql
├── docker-compose.yml
└── Dockerfile (for backend)
```

---
## Run this script to have the structure created automatically
```sh
#!/bin/bash

# Create directories
mkdir -p myapp/backend myapp/frontend myapp/db
cd myapp

# Create backend/app.py
cat <<EOF > backend/app.py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello from the Backend Flask App!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
EOF

# Create frontend/index.html
cat <<EOF > frontend/index.html
<!DOCTYPE html>
<html>
<head>
    <title>Frontend</title>
</head>
<body>
    <h1>Hello from Frontend</h1>
</body>
</html>
EOF

# Create db/init.sql
cat <<EOF > db/init.sql
CREATE TABLE IF NOT EXISTS messages (
    id SERIAL PRIMARY KEY,
    text VARCHAR(255) NOT NULL
);
EOF

# Create Dockerfile (for backend)
cat <<EOF > Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY backend/ .

RUN pip install flask

CMD ["python", "app.py"]
EOF

# Create docker-compose.yml
cat <<EOF > docker-compose.yml
version: "3.9"

services:
  backend:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db

  frontend:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./frontend:/usr/share/nginx/html:ro

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypass
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql:ro

volumes:
  db_data:
EOF

echo "✅ Project structure created successfully in ./myapp"
```
## 📜 Sample `docker-compose.yml`

```yaml
version: "3.9"

services:
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - db

  frontend:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./frontend:/usr/share/nginx/html

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypass
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

---

## 🚀 Common Docker Compose Commands

```bash
docker-compose up               # Start all services
docker-compose up -d            # Run in detached mode
docker-compose down             # Stop and remove all containers
docker-compose ps               # List running services
docker-compose logs             # View logs for all services
docker-compose build            # Build services
docker-compose restart backend  # Restart a specific service
```

---

## 🔄 Rebuild and Restart

```bash
docker-compose down --volumes   # Remove all volumes too
docker-compose up --build       # Rebuild and start
```

---

## 🧪 Compose for Local Development

Use Compose to simulate a full application stack locally:

- Backend (e.g., Flask/Django/Node)
- Frontend (e.g., React/Vue/Angular)
- Database (e.g., PostgreSQL/MySQL)
- Proxy/Load balancer (e.g., NGINX)

---

## 🧠 Tips and Best Practices

- Use `.env` files for secrets and environment config  
- Use `depends_on` for startup order (not readiness checks)  
- Bind volumes for live code changes  
- Version-lock your base images  

---

> ✅ Docker Compose is a must-have for developing, testing, and deploying multi-container apps with consistency.
