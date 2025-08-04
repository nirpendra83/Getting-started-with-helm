---
title: "Deploy a .NET Core Web API using Docker and Kubernetes"
date: 2025-08-01
description: "Step-by-step guide to build, containerize, and deploy a .NET Core Web API to a Kubernetes cluster using Docker."
tags: ["dotnet", "kubernetes", "docker", "webapi", "devops"]
draft: false
weight: 10
---

## Required softwares 
- Vscode(optional)
- Gitbash
- putty or mobaxterm
- Docker Desktop
   -  Check if your docker desktop using Linux OS or WIndows os
    ```sh
    docker info | findstr /C:"OSType"
    OSType: windows
    ```
   - How to swith b/w OS: (Run this as an admin)
   ```sh
   & "$Env:ProgramFiles\Docker\Docker\DockerCli.exe" -SwitchDaemon
   ```

## Prerequisites to install docker and kubernetes

- Linux system (Ubuntu 20.04/22.04, RHEL 8/9, or Rocky Linux)
- User with `sudo` privileges
- Internet connection
- Docker Hub account (or private container registry)

---

## Step 1: Install .NET 8 SDK on Linux

### Ubuntu 22.04 / 20.04

```bash
sudo apt update
sudo apt install -y wget apt-transport-https software-properties-common
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb


```

- ** Install Dot net version 8
```bash
sudo apt update
sudo apt install -y dotnet-sdk-8.0
```
- **Verify**
```bash
dotnet --version
```
### Step 2: Install Docker and Kubernetes Tools
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor | sudo tee /etc/apt/keyrings/docker.gpg > /dev/null

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker
sudo systemctl start docker
```

### Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

```
### Step 3: Create .NET Core Web API Project
```bash
dotnet new webapi -n HelloWorldApi
cd HelloWorldApi
dotnet run
```

- **Add some code to confirm it's running**
```sh
In Program.cs or Startup.cs, add:
Console.WriteLine("App started");

```
- **Now check again**
```sh
dotnet run
```

### Create a Dockerfile
```sh
Dockerfile 
# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy the rest of the source and build
COPY . ./
RUN dotnet publish -c Release -o /app/out

# Stage 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/out .

# Expose the default port
EXPOSE 80

# Run the application
ENTRYPOINT ["dotnet", "HelloWorldApi.dll"]

```
### . Build the Docker image:
```sh
docker build -t helloworldapi:8 .
```
### Run the container
```sh
docker run -d -p 8080:5000 --name hwapi helloworldapi:8
```
- Steps to Run .NET 8 Web App Persistently in Docker
In Program.cs, ensure your app listens on 0.0.0.0:
```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello from .NET 8 API");
app.Run("http://0.0.0.0:5000");
```
- Again create an image 
```sh
docker build -t helloworldapi:8 .
``
- Run the container now
```sh

docker run -d --restart=always -p 8080:5000 --name hwapi helloworldapi:8
```
- Access your app from browser
```sh
ip:8080
```

### Now push this image to dockerhub
```sh
docker login
```
### Tag your image as per your user
```sh
docker tag helloworldapi:8  nippy/helloworldapi:8
```
### push your image to dockerhub
```sh
docker push nippy/helloworldapi:8
```
### Deploy this to kubernetes (Create deployment )
```sh 
helloworld-deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-api
spec:
  replicas: 1
  selector:
    matchLabels:
      app: helloworld-api
  template:
    metadata:
      labels:
        app: helloworld-api
    spec:
      containers:
      - name: helloworld-api
        image: nippy/helloworldapi:8
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
```
### ✅ 2. Service YAML (unchanged, helloworld-svc.yaml)
```sh
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  type: LoadBalancer
  selector:
    app: helloworld-api
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
 ```    


- **OR**
```sh
kubectl expose deployment helloworld-api --port 80 --type LoadBalancer --target-port 5000  --name test01
 ```
### 🚀 3. Deploy to Kubernetes
```sh
kubectl apply -f helloworld-deploy.yaml
kubectl apply -f helloworld-svc.yaml
```
### Now check the loadBalancer IP
```sh
kubectl get svc
```
### Access in Browser
```sh
ip:80
```




###  3 Tier APP ############
### Complete Sample: Angular Frontend + .NET 8 Web API + PostgreSQL + Docker Compose

## Step 1: Folder Structure
```
my-fullstack-app/
├── backend/
│   ├── Controllers/
│   ├── Data/
│   ├── Models/
│   ├── Program.cs
│   ├── appsettings.json
│   ├── Dockerfile
│   └── HelloWorldApi.csproj
├── frontend/
│   ├── (Angular app here)
│   └── Dockerfile
└── docker-compose.yml
```

---

## Step 2: Backend - .NET 8 Web API

### `Program.cs`
```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using HelloWorldApi.Data;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseAuthorization();
app.MapControllers();
app.Run();
```

### `Data/AppDbContext.cs`
```csharp
using Microsoft.EntityFrameworkCore;
using HelloWorldApi.Models;

namespace HelloWorldApi.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) {}

        public DbSet<Message> Messages { get; set; }
    }
}
```

### `Models/Message.cs`
```csharp
namespace HelloWorldApi.Models
{
    public class Message
    {
        public int Id { get; set; }
        public string Text { get; set; }
    }
}
```

### `Controllers/MessageController.cs`
```csharp
using Microsoft.AspNetCore.Mvc;
using HelloWorldApi.Data;
using HelloWorldApi.Models;
using Microsoft.EntityFrameworkCore;
using System.Threading.Tasks;
using System.Collections.Generic;

namespace HelloWorldApi.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class MessageController : ControllerBase
    {
        private readonly AppDbContext _context;

        public MessageController(AppDbContext context)
        {
            _context = context;
        }

        [HttpGet]
        public async Task<IEnumerable<Message>> GetMessages() => await _context.Messages.ToListAsync();

        [HttpPost]
        public async Task<IActionResult> CreateMessage(Message msg)
        {
            _context.Messages.Add(msg);
            await _context.SaveChangesAsync();
            return CreatedAtAction(nameof(GetMessages), new { id = msg.Id }, msg);
        }
    }
}
```

### `appsettings.json`
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=postgres;Port=5432;Database=helloapi;Username=postgres;Password=postgres"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### `Dockerfile` (backend)
```Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5000

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "HelloWorldApi.dll"]
```

---

## Step 3: Frontend - Angular App
Generate Angular app:
```bash
ng new frontend --routing=false --style=css
cd frontend
ng generate component home
```

### `src/app/app.component.html`
```html
<h2>Messages</h2>
<ul>
  <li *ngFor="let message of messages">{{ message.text }}</li>
</ul>
<input [(ngModel)]="newMessage" />
<button (click)="addMessage()">Add</button>
```

### `src/app/app.component.ts`
```ts
import { HttpClient } from '@angular/common/http';
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent {
  messages: any[] = [];
  newMessage: string = '';

  constructor(private http: HttpClient) {
    this.loadMessages();
  }

  loadMessages() {
    this.http.get<any[]>('http://localhost:8080/api/message').subscribe(data => this.messages = data);
  }

  addMessage() {
    this.http.post('http://localhost:8080/api/message', { text: this.newMessage }).subscribe(() => {
      this.newMessage = '';
      this.loadMessages();
    });
  }
}
```

### `Dockerfile` (frontend)
```Dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build -- --output-path=dist

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
```

### `nginx.conf`
```nginx
server {
  listen 80;
  location / {
    root /usr/share/nginx/html;
    index index.html;
    try_files $uri $uri/ /index.html;
  }
  location /api/ {
    proxy_pass http://backend:5000/api/;
  }
}
```

---

## Step 4: Docker Compose
### `docker-compose.yml`
```yaml
version: '3.9'

services:
  frontend:
    build: ./frontend
    ports:
      - "8080:80"
    depends_on:
      - backend

  backend:
    build: ./backend
    environment:
      - ASPNETCORE_URLS=http://+:5000
    ports:
      - "5000:5000"
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: helloapi
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

---

## Step 5: Build & Run
```bash
docker-compose up --build -d
```

Then visit:
- Angular UI: http://localhost:8080
- Swagger (API docs): http://localhost:5000/swagger
