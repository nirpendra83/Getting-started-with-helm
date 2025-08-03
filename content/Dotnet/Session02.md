---
title: "Deploying Learning Portal with Docker and Kubernetes"
date: 2025-08-01
description: "Step-by-step guide to containerize and deploy a .NET Core + Angular + PostgreSQL application using Docker and Kubernetes"
---

## Overview
This guide explains how to containerize and deploy the Learning Portal application (https://github.com/Ashish-Ranjan001/Learning-Portal-) using Docker and Kubernetes. The application is composed of:

- **Frontend**: Angular
- **Backend**: .NET Core
- **Database**: PostgreSQL

---

## Prerequisites
- Docker installed
- Kubernetes cluster (e.g., Minikube, CRC, K3s, or cloud-managed)
- `kubectl` configured
- Docker Hub account (e.g., `nippy`)

---

## Step 1: Clone the Repository
```bash
git clone https://github.com/Ashish-Ranjan001/Learning-Portal-.git
cd Learning-Portal-
```
## Step 2: Prepare the Directory Structure
```sh
Learning-Portal-/
├── frontend/         # Angular
├── backend/          # .NET Core
├── docker-compose.yml
```
## Step 3: Dockerize the Backend (.NET Core)

Create backend/Dockerfile:
```sh
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "LearningPortal.dll"]
```
## Step 4: Dockerize the Frontend (Angular)

Create frontend/Dockerfile:
```sh
FROM node:18-alpine as build
WORKDIR /app
COPY . .
RUN npm install && npm run build --prod

FROM nginx:alpine
COPY --from=build /app/dist/* /usr/share/nginx/html
```
## Step 5: Create docker-compose.yml
```sh
version: '3.8'
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: learning
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - appnet

  backend:
    build: ./backend
    depends_on:
      - db
    environment:
      - DB_HOST=db
      - DB_USER=user
      - DB_PASS=password
      - DB_NAME=learning
    networks:
      - appnet

  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend
    networks:
      - appnet

volumes:
  pgdata:

networks:
  appnet:
```
## Step 6: Build and Push Docker Images
```sh
# Backend
cd backend
docker build -t nippy/learning-backend:latest .
docker push nippy/learning-backend:latest

# Frontend
cd ../frontend
docker build -t nippy/learning-frontend:latest .
docker push nippy/learning-frontend:latest
```