## Technical Implementation Steps

This section describes the concrete technical steps taken to build the project,
from initial setup to the final Dockerized architecture.

---

## Step 1: Static Websites Creation

Three simple static websites were created to simulate independent services:

- `site1`
- `site2`
- `site3`

Each site contains:
- an `index.html`
- minimal static content for identification

Purpose:
- to represent separate backend services
- to verify routing and isolation later on

---

## Step 2: Nginx Reverse Proxy Configuration

An Nginx configuration was created to act as a reverse proxy.

Responsibilities:
- listen on port `80`
- forward traffic to backend services based on routing rules
- hide internal service ports from the client

Key concept:
- Nginx acts as the **single entry point**
- backend services remain private and isolated

---

## Step 3: Containerizing the Backend Services

Each site was containerized using Docker.

For every site:
- a `Dockerfile` was created
- a lightweight web server was used
- the site content was copied into the image

Images created:
- `site1-image`
- `site2-image`
- `site3-image`

This step ensured:
- reproducible builds
- consistent environments
- easy scaling

---

## Step 4: Containerizing the Nginx Proxy

Nginx was also containerized as a separate service.

The proxy image:
- includes a custom Nginx configuration
- forwards requests to the site containers
- exposes port `80`

Image created:
- `proxy-image`

This completes the container-based architecture.

---

## Step 5: Image Tagging and Versioning

All images were tagged using semantic versioning:

- `v1.0.0` – first stable release

Tagging provides:
- clear release tracking
- rollback capability
- predictable deployments

---

## Step 6: Publishing Images to Docker Hub

The Docker images were pushed to Docker Hub.

Purpose:
- centralized image storage
- easy deployment on any server
- separation of code (GitHub) and artifacts (Docker Hub)

---

## Step 7: Source Control and History Tracking

The entire project:
- source code
- Dockerfiles
- Nginx configuration
- documentation

was committed and pushed to GitHub to preserve:
- full development history
- transparency of changes
- reproducibility



## Commands Used (Build, Run, Push)

This section documents the exact commands used during the development
to ensure full reproducibility of the project.

---

## Build Docker Images

Each service was built into its own Docker image.

```bash
docker build -t site1-image ./site1
docker build -t site2-image ./site2
docker build -t site3-image ./site3
docker build -t proxy-image ./proxy

Purpose:

    create isolated images for each service

    ensure consistent builds

Run Containers Locally

Containers were started with explicit port mappings for testing.

docker run -d -p 8001:80 site1-image
docker run -d -p 8002:80 site2-image
docker run -d -p 8003:80 site3-image
docker run -d -p 80:80 proxy-image

Result:

    services accessible internally

    proxy exposed as the main entry point

Verify Services

Services were tested using both browser and CLI.

curl http://localhost
curl http://localhost/site1
curl http://localhost/site2
curl http://localhost/site3

Tag Images for Docker Hub

Images were tagged using semantic versioning.

docker tag site1-image <dockerhub-username>/site1-image:v1.0.0
docker tag site2-image <dockerhub-username>/site2-image:v1.0.0
docker tag site3-image <dockerhub-username>/site3-image:v1.0.0
docker tag proxy-image <dockerhub-username>/proxy-image:v1.0.0

Push Images to Docker Hub

After authentication, images were pushed to Docker Hub.

docker login
docker push <dockerhub-username>/site1-image:v1.0.0
docker push <dockerhub-username>/site2-image:v1.0.0
docker push <dockerhub-username>/site3-image:v1.0.0
docker push <dockerhub-username>/proxy-image:v1.0.0

Purpose:

    centralized image storage

    portability across environments

Docker Compose Setup

Docker Compose is used to run all services together in a single network.

docker-compose.yml
version: "3.9"

services:
  site1:
    image: eritest/site1-image:v1.0.0
    container_name: site1
    restart: always

  site2:
    image: eritest/site2-image:v1.0.0
    container_name: site2
    restart: always

  site3:
    image: eritest/site3-image:v1.0.0
    container_name: site3
    restart: always

  proxy:
    image: eritest/proxy-image:v1.0.0
    container_name: proxy
    ports:
      - "80:80"
    depends_on:
      - site1
      - site2
      - site3
    restart: always

How to Run the Project
1. Prerequisites

Docker installed

Docker Compose installed

2. Start all services
docker-compose up -d

3. Access from browser

http://localhost/site1

http://localhost/site2

http://localhost/site3

4. Stop the project
docker-compose down

Docker Hub Images

All images are published to Docker Hub:

eritest/site1-image:v1.0.0

eritest/site2-image:v1.0.0

eritest/site3-image:v1.0.0

eritest/proxy-image:v1.0.0

This allows the project to be started without local builds.

Key Concepts Learned

Difference between images and containers

Docker networking and DNS resolution

Reverse proxy fundamentals

Service orchestration with Docker Compose

Versioning and tagging Docker images

Publishing images to Docker Hub

Maintaining project history with GitHub
