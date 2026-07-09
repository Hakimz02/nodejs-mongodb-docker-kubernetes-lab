# nodejs-mongodb-docker-kubernetes-lab
Hands-on lab for deploying a Node.js and MongoDB application using Docker and Kubernetes.

# Node.js and MongoDB Deployment Lab using Docker and Kubernetes

This project documents my hands-on deployment of a two-tier Node.js Pac-Man application with MongoDB using Docker and Kubernetes.

The goal of this lab was to understand basic application deployment, Docker container networking, database connectivity, port exposure, and a simple Kubernetes deployment using Docker Desktop.

## Project Overview

The application consists of:

- Node.js web application
- MongoDB database
- Docker deployment on Ubuntu VM
- Kubernetes deployment on Docker Desktop

The Node.js application connects to MongoDB to store user stats and high scores.

## Technologies Used

- Ubuntu Server
- SSH
- Docker
- Dockerfile
- Docker Network
- MongoDB 3.4
- Node.js Boron 6.x
- Kubernetes
- Docker Desktop Kubernetes
- kubectl
- NodePort Service

## Docker Deployment Flow

The Docker deployment used two separate containers:

- `pacman-app` for the Node.js web application
- `pacman-mongo` for MongoDB

Both containers were connected to the same Docker network so the application could reach MongoDB using the container name.

```text
Browser
  ↓
VM Public IP : 8080
  ↓
pacman-app container
  ↓
Docker network
  ↓
pacman-mongo container
  ↓
MongoDB database
