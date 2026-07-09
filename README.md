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

Both containers were connected to the same Docker network so the application could reach MongoDB using the container name `pacman-mongo`.

```text
Browser
  ↓
Ubuntu VM port 8080
  ↓
pacman-app container
  ↓
Docker network: pacman-net
  ↓
pacman-mongo container
  ↓
MongoDB database
```

## Key Docker Steps

Created a Docker network:

```bash
sudo docker network create pacman-net
```

Started MongoDB 3.4:

```bash
sudo docker run -d --name pacman-mongo --network pacman-net --restart unless-stopped mongo:3.4
```

Built the Node.js application image:

```bash
sudo docker build -t pacman-nodejs-app -f ./docker/Dockerfile .
```

Started the application container with MongoDB environment variables:

```bash
sudo docker run -d \
  --name pacman-app \
  --network pacman-net \
  -p 8080:8080 \
  --restart unless-stopped \
  -e MONGO_SERVICE_HOST=pacman-mongo \
  -e MY_MONGO_PORT=27017 \
  -e MONGO_DATABASE=pacman \
  pacman-nodejs-app
```

Checked running containers:

```bash
sudo docker ps
```

## Verification

The application was tested through the browser and the Pac-Man game loaded successfully.

The high score function was also tested and verified in MongoDB:

```bash
sudo docker exec pacman-mongo mongo pacman --quiet --eval 'db.highscore.find().pretty()'
```

This confirmed that the Node.js application was able to connect to MongoDB and save high score data.

## Kubernetes Bonus Deployment

I also deployed the same application and database on a local Kubernetes cluster using Docker Desktop.

The Kubernetes deployment used:

- MongoDB Deployment
- MongoDB Service
- Pac-Man App Deployment
- Pac-Man App NodePort Service

The application was exposed locally using NodePort on port `30080`.

```text
Browser
  ↓
localhost:30080
  ↓
pacman-app Service
  ↓
pacman-app Pod
  ↓
pacman-mongo Service
  ↓
pacman-mongo Pod
```

## Kubernetes Verification

Checked cluster node:

```bash
kubectl get nodes
```

Checked running pods:

```bash
kubectl get pods
```

Checked services:

```bash
kubectl get svc
```

The Pac-Man application pod and MongoDB pod were both running successfully, and the application was accessible through:

```text
http://localhost:30080
```

## What I Learned

From this lab, I learned:

- How to deploy a Node.js application with MongoDB
- How Docker containers communicate using a Docker network
- How to use environment variables for database connection configuration
- How to expose a containerized web application using port mapping
- How to verify data persistence in MongoDB
- Basic Kubernetes concepts such as Pod, Deployment, Service, and NodePort
- The difference between running containers manually with Docker and managing them with Kubernetes

## Notes

This project is for learning and portfolio documentation. The Kubernetes setup uses local Docker Desktop Kubernetes and does not include persistent volume storage for MongoDB.
