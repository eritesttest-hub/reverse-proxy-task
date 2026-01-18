# Docker Reverse Proxy Project

This project demonstrates a Docker-based setup where multiple
static websites are served behind an Nginx reverse proxy.

The goal of this project is to understand how Docker containers
communicate internally and how a single entry point can route
traffic to different services

## Architecture

The project consists of:

- Three containers serving static content (site1, site2, site3)
- One Nginx container acting as a reverse proxy
- All containers connected through a Docker network
- Only the Nginx container exposes port 80 to the host

## Routing

Traffic is routed based on the request path:

- `/site1` → site1 container
- `/site2` → site2 container
- `/site3` → site3 container
## Technologies used

- Docker
- Docker Compose
- Nginx

