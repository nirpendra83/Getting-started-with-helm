---
title: ".NET Related containers"
draft: false
tags: ["docker", "dockerfile", "containers", "devops", "tutorial"]
---


### Install ms sql on container 
```sh
docker run -e "ACCEPT_EULA=Y" \
           -e "MSSQL_SA_PASSWORD=Redhat@123456" \
           -p 1433:1433 \
           --name sql2022 \
           -d mcr.microsoft.com/mssql/server:2022-latest
```
### Create a database in Ms sql
```sh
docker run -it --rm \
  --network container:sql2022 \
  mcr.microsoft.com/mssql-tools \
  /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 'Redhat@123456' -Q "CREATE DATABASE LMSDatabase"
```
### Check the database if available
```sh
 docker run -it --rm   --network container:sql2022   mcr.microsoft.com/mssql-tools   /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 'Redhat@123456'

1> SELECT name FROM sys.databases;
2> GO
name
--------------------------------------------------------------------------------------------------------------------------------
master
tempdb
model
msdb
LMSDatabase

(5 rows affected)
```

```sh
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost,1433;Database=LMSDatabase;User Id=sa;Password=Redhat@123456;TrustServerCertificate=true"
  }
}
```

## 🚀 Host a Static Website with IIS in Docker (Windows)

This guide walks you through creating and deploying a simple static HTML site using **IIS** inside a **Docker container** on **Windows**.

---

## 📁 Project Structure

```
iis-static-site/
├── Dockerfile
└── html/
    └── index.html
```

---

## 📝 Step 1: Create `index.html`

**`html/index.html`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Welcome to IIS Container</title>
</head>
<body>
  <h1>Hello from Docker + IIS!</h1>
  <p>This static page is served from an IIS container.</p>
</body>
</html>
```

---

## 🐳 Step 2: Create `Dockerfile`

**`Dockerfile`**
```Dockerfile
# Use the official IIS image
FROM mcr.microsoft.com/windows/servercore/iis

# Copy static site contents to IIS web root
COPY ./html/ /inetpub/wwwroot/
```

---

## 🏗️ Step 3: Build Docker Image

Open PowerShell or Command Prompt in the project root and run:

```powershell
docker build -t iis-static-site .
```

---

## ▶️ Step 4: Run the Container

```powershell
docker run -d -p 8080:80 --name iis-container iis-static-site
```

---

## 🌐 Step 5: Access the Website

Visit the following URL in your browser:

```
http://localhost:8080
```

You should see:

> ✅ Hello from Docker + IIS!

---

## 🧹 Step 6: Stop & Remove Container (Optional)

```powershell
docker stop iis-container
docker rm iis-container
```

---






https://learn.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=linux&pivots=dotnet-8-0
Some Example:
https://github.com/nitin27may/clean-architecture-docker-dotnet-angular
https://github.com/trevoirwilliams/ConferenceAttendees.CloudNativeDevelopment.git
https://learn.microsoft.com/en-us/visualstudio/containers/tutorial-multicontainer?view=vs-2022

https://ravindradevrani.medium.com/containerize-your-net-application-with-sql-server-using-docker-compose-d04b0c4ff4d1

https://github.com/docker/awesome-compose/tree/master/aspnet-mssql