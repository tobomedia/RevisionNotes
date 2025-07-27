# Docker & Docker Compose Cheatsheet

*A comprehensive reference for Docker containerization and orchestration*

📚 **Official Documentation:** <a href="https://docs.docker.com/" target="_blank">Docker Documentation</a>  
🐳 **Docker Hub:** <a href="https://hub.docker.com/" target="_blank">Official Image Registry</a>

## Table of Contents

### 🚀 Getting Started
- [Installation & Setup](#installation--setup)
- [Docker Basics](#docker-basics)
- [Docker Images](#docker-images)
- [Docker Containers](#docker-containers)

### 🔧 Core Docker Commands
- [Image Management](#image-management)
- [Container Management](#container-management)
- [Volume Management](#volume-management)
- [Network Management](#network-management)

### 📝 Dockerfile
- [Dockerfile Basics](#dockerfile-basics)
- [Dockerfile Instructions](#dockerfile-instructions)
- [Multi-stage Builds](#multi-stage-builds)
- [Best Practices](#dockerfile-best-practices)

### 🐳 Docker Compose
- [Docker Compose Basics](#docker-compose-basics)
- [Compose File Structure](#compose-file-structure)
- [Services Configuration](#services-configuration)
- [Compose Commands](#compose-commands)

### 🏗️ Advanced Topics
- [Docker Registry](#docker-registry)
- [Docker Swarm](#docker-swarm)
- [Security](#security)
- [Monitoring & Logging](#monitoring--logging)

### 📋 Best Practices & Patterns
- [Production Deployment](#production-deployment)
- [Development Workflow](#development-workflow)
- [Common Patterns](#common-patterns)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## Installation & Setup

📖 <a href="https://docs.docker.com/get-docker/" target="_blank">Installation Documentation</a>

```bash
# Install Docker (Ubuntu/Debian)
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add user to docker group (avoid sudo)
sudo usermod -aG docker $USER
newgrp docker

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verify installation
docker --version
docker-compose --version
docker run hello-world

# Configure Docker daemon
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

## Docker Basics

📖 <a href="https://docs.docker.com/get-started/overview/" target="_blank">Docker Overview</a>

```bash
# Basic Docker workflow
docker pull image_name          # Download image
docker run image_name           # Create and start container
docker ps                       # List running containers
docker stop container_id        # Stop container
docker rm container_id          # Remove container

# Get help
docker --help
docker run --help
docker-compose --help

# Docker system information
docker version                 # Docker version info
docker info                    # System-wide information
docker system df               # Show docker disk usage
docker system prune            # Remove unused data

# Docker configuration
docker config ls               # List configs
docker context ls              # List contexts
docker context use context_name # Switch context
```

## Docker Images

📖 <a href="https://docs.docker.com/engine/reference/commandline/image/" target="_blank">Images Documentation</a>

```bash
# List images
docker images
docker image ls
docker image ls --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

# Pull images
docker pull ubuntu             # Latest tag
docker pull ubuntu:20.04      # Specific tag
docker pull ubuntu@sha256:abc  # Specific digest

# Search images
docker search nginx
docker search --limit 5 nginx

# Build images
docker build .                 # Build from current directory
docker build -t myapp:latest . # Build with tag
docker build -f Dockerfile.dev . # Specify Dockerfile
docker build --no-cache .      # Build without cache

# Tag images
docker tag source_image:tag target_image:tag
docker tag myapp:latest myapp:v1.0
docker tag myapp:latest registry.com/myapp:latest

# Push images
docker push myapp:latest
docker push registry.com/myapp:latest

# Remove images
docker rmi image_id            # Remove image
docker rmi $(docker images -q) # Remove all images
docker image prune             # Remove dangling images
docker image prune -a          # Remove all unused images

# Inspect images
docker inspect image_name      # Detailed image info
docker history image_name      # Show image layers
```

## Docker Containers

📖 <a href="https://docs.docker.com/engine/reference/commandline/container/" target="_blank">Containers Documentation</a>

```bash
# Run containers
docker run ubuntu                    # Run and exit
docker run -it ubuntu bash          # Interactive terminal
docker run -d nginx                 # Detached mode
docker run --name mycontainer nginx # Named container
docker run -p 8080:80 nginx        # Port mapping
docker run -v /host:/container nginx # Volume mount
docker run --rm nginx               # Auto-remove when stopped

# Advanced run options
docker run -d \
  --name webapp \
  --restart unless-stopped \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -v $(pwd):/app \
  -w /app \
  --memory=512m \
  --cpus=1.5 \
  node:16 npm start

# List containers
docker ps                    # Running containers
docker ps -a                 # All containers
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Container lifecycle
docker start container_name  # Start stopped container
docker stop container_name   # Gracefully stop
docker restart container_name # Restart container
docker kill container_name   # Force stop
docker pause container_name  # Pause container
docker unpause container_name # Unpause container

# Execute commands in containers
docker exec -it container_name bash    # Interactive shell
docker exec container_name ls -la      # Run command
docker exec -u root container_name sh  # Run as different user

# Container information
docker logs container_name              # View logs
docker logs -f container_name           # Follow logs
docker logs --tail 50 container_name    # Last 50 lines
docker stats                           # Live resource usage
docker top container_name              # Running processes
docker inspect container_name          # Detailed info

# Copy files
docker cp file.txt container_name:/path/  # Host to container
docker cp container_name:/path/file.txt . # Container to host

# Remove containers
docker rm container_name               # Remove stopped container
docker rm -f container_name            # Force remove running container
docker container prune                 # Remove all stopped containers
```

## Image Management

📖 <a href="https://docs.docker.com/develop/dev-best-practices/" target="_blank">Image Management Best Practices</a>

```bash
# Image cleanup
docker image prune                     # Remove dangling images
docker image prune -a                  # Remove all unused images
docker image prune --filter "until=24h" # Remove images older than 24h

# Export/Import images
docker save myapp:latest > myapp.tar   # Export image to tar
docker load < myapp.tar                # Import image from tar
docker export container_name > container.tar # Export container
docker import container.tar myapp:imported   # Import as image

# Image scanning (Docker Desktop)
docker scan myapp:latest               # Scan for vulnerabilities

# Multi-architecture builds
docker buildx create --name multiarch  # Create builder
docker buildx use multiarch            # Use builder
docker buildx build --platform linux/amd64,linux/arm64 -t myapp:latest --push .

# Image layers and cache
docker build --target development .    # Build specific stage
docker build --cache-from myapp:cache . # Use external cache
```

## Container Management

📖 <a href="https://docs.docker.com/engine/reference/run/" target="_blank">Container Runtime Options</a>

```bash
# Resource constraints
docker run --memory=512m nginx        # Limit memory
docker run --cpus=1.5 nginx          # Limit CPU
docker run --device=/dev/sda nginx    # Device access

# Environment variables
docker run -e ENV_VAR=value nginx     # Single variable
docker run --env-file .env nginx      # From file

# Health checks
docker run --health-cmd="curl -f http://localhost || exit 1" \
           --health-interval=30s \
           --health-timeout=3s \
           --health-retries=3 \
           nginx

# Update containers
docker update --restart=always container_name # Update restart policy
docker update --memory=1g container_name      # Update memory limit

# Container diff
docker diff container_name             # Show filesystem changes

# Commit containers to images
docker commit container_name new_image:tag
docker commit -m "Added feature" container_name new_image:tag
```

## Volume Management

📖 <a href="https://docs.docker.com/storage/volumes/" target="_blank">Volume Documentation</a>

```bash
# Create and manage volumes
docker volume create myvolume         # Create named volume
docker volume ls                      # List volumes
docker volume inspect myvolume        # Inspect volume
docker volume rm myvolume            # Remove volume
docker volume prune                   # Remove unused volumes

# Use volumes
docker run -v myvolume:/data nginx    # Named volume
docker run -v /host/path:/container/path nginx # Bind mount
docker run -v /container/path nginx   # Anonymous volume

# Volume types
# Bind mounts
docker run -v "$(pwd)":/app nginx     # Current directory
docker run -v /etc/nginx:/etc/nginx:ro nginx # Read-only

# tmpfs mounts (memory)
docker run --tmpfs /tmp nginx         # Memory-based filesystem

# Volume drivers
docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.1,rw \
  --opt device=:/path/to/dir \
  nfs-volume

# Backup and restore volumes
docker run --rm -v myvolume:/data -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz -C /data .
docker run --rm -v myvolume:/data -v $(pwd):/backup alpine tar xzf /backup/backup.tar.gz -C /data
```

## Network Management

📖 <a href="https://docs.docker.com/network/" target="_blank">Networking Documentation</a>

```bash
# List and inspect networks
docker network ls                     # List networks
docker network inspect bridge         # Inspect network
docker network inspect container_name # Container network info

# Create networks
docker network create mynetwork       # Bridge network
docker network create --driver overlay myoverlay # Overlay network
docker network create --subnet=172.20.0.0/16 mynetwork # Custom subnet

# Network types
docker network create \
  --driver bridge \
  --subnet=172.20.0.0/16 \
  --ip-range=172.20.240.0/20 \
  --gateway=172.20.0.1 \
  custom-bridge

# Connect containers to networks
docker run --network mynetwork nginx  # Connect at runtime
docker network connect mynetwork container_name # Connect existing container
docker network disconnect mynetwork container_name # Disconnect

# Container communication
docker run --name web nginx          # Container named 'web'
docker run --link web:web app        # Link to web container (legacy)

# Modern approach with custom networks
docker network create app-network
docker run --name web --network app-network nginx
docker run --name app --network app-network myapp
# Now 'app' can reach 'web' by name

# Port publishing
docker run -p 8080:80 nginx          # Host port 8080 to container port 80
docker run -p 127.0.0.1:8080:80 nginx # Bind to specific interface
docker run -P nginx                   # Publish all exposed ports

# Remove networks
docker network rm mynetwork          # Remove network
docker network prune                 # Remove unused networks
```

## Dockerfile Basics

📖 <a href="https://docs.docker.com/engine/reference/builder/" target="_blank">Dockerfile Reference</a>

```dockerfile
# Basic Dockerfile structure
FROM node:16-alpine                   # Base image

# Set metadata
LABEL maintainer="you@example.com"
LABEL version="1.0"
LABEL description="My Node.js app"

# Set working directory
WORKDIR /app

# Copy files
COPY package*.json ./                 # Copy package files
COPY . .                             # Copy all files

# Install dependencies
RUN npm ci --only=production && \
    npm cache clean --force

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001

# Change ownership
RUN chown -R nextjs:nodejs /app
USER nextjs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Set environment variables
ENV NODE_ENV=production
ENV PORT=3000

# Define entrypoint and command
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["npm", "start"]
```

## Dockerfile Instructions

📖 <a href="https://docs.docker.com/develop/dev-best-practices/" target="_blank">Dockerfile Best Practices</a>

```dockerfile
# FROM - Base image
FROM ubuntu:20.04                     # Specific version
FROM node:16-alpine AS builder        # Multi-stage with name
FROM scratch                          # Empty base image

# RUN - Execute commands
RUN apt-get update && apt-get install -y \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*    # Clean up in same layer

# COPY vs ADD
COPY src/ /app/src/                   # Copy files/directories
ADD https://example.com/file.tar.gz /tmp/ # Can download and extract

# WORKDIR - Set working directory
WORKDIR /app                          # Creates directory if it doesn't exist

# ENV - Environment variables
ENV NODE_ENV=production
ENV PATH="/app/bin:${PATH}"

# ARG - Build-time variables
ARG VERSION=latest
ARG BUILD_DATE
RUN echo "Building version ${VERSION} on ${BUILD_DATE}"

# EXPOSE - Document ports
EXPOSE 3000
EXPOSE 8080/tcp
EXPOSE 53/udp

# VOLUME - Mount points
VOLUME ["/data", "/logs"]

# USER - Set user context
USER 1001
USER nodejs:nodejs

# SHELL - Override default shell
SHELL ["/bin/bash", "-c"]

# ONBUILD - Trigger for child images
ONBUILD COPY . /app
ONBUILD RUN npm install

# STOPSIGNAL - Signal to stop container
STOPSIGNAL SIGTERM

# Conditional instructions
ARG ENVIRONMENT=production
RUN if [ "$ENVIRONMENT" = "development" ]; then \
      npm install; \
    else \
      npm ci --only=production; \
    fi
```

## Multi-stage Builds

📖 <a href="https://docs.docker.com/develop/dev-best-practices/#use-multi-stage-builds" target="_blank">Multi-stage Builds Documentation</a>

```dockerfile
# Multi-stage build example
# Stage 1: Build environment
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Production environment
FROM node:16-alpine AS production
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force
COPY --from=builder /app/dist ./dist
USER node
EXPOSE 3000
CMD ["npm", "start"]

# Stage 3: Development environment
FROM node:16-alpine AS development
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
USER node
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Build specific stage
# docker build --target development -t myapp:dev .
# docker build --target production -t myapp:prod .

# Advanced multi-stage with external image
FROM golang:1.19-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

FROM alpine:latest AS production
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]

# Copy from external image
FROM nginx:alpine
COPY --from=node:16-alpine /usr/local/bin/node /usr/local/bin/
```

## Dockerfile Best Practices

📖 <a href="https://docs.docker.com/develop/dev-best-practices/" target="_blank">Dockerfile Best Practices Guide</a>

```dockerfile
# 1. Use specific base image versions
FROM node:16.14.2-alpine           # Good - specific version
FROM node:latest                   # Bad - unpredictable

# 2. Minimize layers and combine RUN commands
RUN apt-get update && \
    apt-get install -y package1 package2 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 3. Order instructions by frequency of change
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./              # Changes less frequently
RUN npm ci --only=production
COPY . .                          # Changes more frequently

# 4. Use .dockerignore
# .dockerignore file:
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
coverage
.nyc_output

# 5. Run as non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001
USER nextjs

# 6. Use COPY instead of ADD (unless you need ADD features)
COPY requirements.txt .            # Good
ADD requirements.txt .             # Unnecessary

# 7. Leverage build cache
COPY package*.json ./              # Copy dependencies first
RUN npm install                    # Cache this layer
COPY . .                          # Copy source code

# 8. Use multi-stage builds
FROM node:16 AS builder
# Build steps...

FROM node:16-alpine
COPY --from=builder /app/dist ./dist

# 9. Set proper labels
LABEL org.opencontainers.image.title="My App"
LABEL org.opencontainers.image.description="A sample application"
LABEL org.opencontainers.image.version="1.0.0"
LABEL org.opencontainers.image.authors="you@example.com"

# 10. Use health checks
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# 11. Optimize for caching
# Group related commands
RUN apt-get update && apt-get install -y \
    git \
    curl \
    vim \
    && rm -rf /var/lib/apt/lists/*

# 12. Use appropriate base images
FROM alpine:latest                 # Minimal
FROM ubuntu:20.04                  # Full-featured
FROM scratch                       # Empty (for static binaries)
FROM distroless/java:11            # Minimal with runtime only
```

## Docker Compose Basics

📖 <a href="https://docs.docker.com/compose/" target="_blank">Compose Documentation</a>

```bash
# Basic Compose commands
docker-compose up                   # Start services
docker-compose up -d                # Start in detached mode
docker-compose up --build           # Build images before starting
docker-compose up service_name      # Start specific service

docker-compose down                 # Stop and remove containers
docker-compose down -v              # Also remove volumes
docker-compose down --rmi all       # Also remove images

docker-compose start                # Start existing containers
docker-compose stop                 # Stop containers
docker-compose restart              # Restart containers

# Service management
docker-compose ps                   # List running services
docker-compose logs                 # View logs
docker-compose logs -f service_name # Follow logs for specific service
docker-compose exec service_name bash # Execute command in service

# Build and push
docker-compose build                # Build all services
docker-compose build service_name   # Build specific service
docker-compose push                 # Push images to registry

# Scaling
docker-compose up --scale web=3     # Scale web service to 3 instances

# Configuration validation
docker-compose config               # Validate and view compose file
docker-compose config --services    # List services
```

## Compose File Structure

📖 <a href="https://docs.docker.com/compose/compose-file/" target="_blank">Compose File Reference</a>

```yaml
# docker-compose.yml
version: '3.8'

services:
  # Web application service
  web:
    build:
      context: .
      dockerfile: Dockerfile
      target: production
      args:
        NODE_ENV: production
    image: myapp:latest
    container_name: myapp-web
    restart: unless-stopped
    ports:
      - "3000:3000"
      - "127.0.0.1:3001:3001"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:pass@db:5432/myapp
    env_file:
      - .env
      - .env.production
    volumes:
      - ./src:/app/src:ro          # Read-only bind mount
      - uploads:/app/uploads       # Named volume
      - /var/log:/app/logs        # Bind mount
    networks:
      - app-network
      - db-network
    depends_on:
      - db
      - redis
    command: ["npm", "start"]
    entrypoint: ["docker-entrypoint.sh"]
    working_dir: /app
    user: "1001:1001"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
    deploy:
      replicas: 2
      resources:
        limits:
          cpus: '1.5'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 256M

  # Database service
  db:
    image: postgres:14-alpine
    container_name: myapp-db
    restart: unless-stopped
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - db-network
    ports:
      - "5432"                     # Internal port only
    secrets:
      - db_password
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  # Redis cache service
  redis:
    image: redis:7-alpine
    container_name: myapp-redis
    restart: unless-stopped
    command: redis-server --appendonly yes
    volumes:
      - redis_data:/data
    networks:
      - app-network
    sysctls:
      - net.core.somaxconn=1024

  # Nginx reverse proxy
  nginx:
    image: nginx:alpine
    container_name: myapp-nginx
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./certs:/etc/nginx/certs:ro
      - static_files:/var/www/static:ro
    networks:
      - app-network
    depends_on:
      - web

# Named volumes
volumes:
  postgres_data:
    driver: local
  redis_data:
    driver: local
  uploads:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /host/uploads
  static_files:
    external: true

# Networks
networks:
  app-network:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16
  db-network:
    driver: bridge
    internal: true               # No external access

# Secrets (Docker Swarm)
secrets:
  db_password:
    file: ./db_password.txt
  ssl_cert:
    external: true

# Configs (Docker Swarm)
configs:
  nginx_config:
    file: ./nginx.conf
```

## Services Configuration

📖 <a href="https://docs.docker.com/compose/compose-file/05-services/" target="_blank">Services Configuration Reference</a>

```yaml
# Advanced service configurations
version: '3.8'

services:
  # Full-featured web service
  web:
    build:
      context: .
      dockerfile: Dockerfile.prod
      target: production
      args:
        - NODE_VERSION=16
        - BUILD_ENV=production
      cache_from:
        - myapp:cache
      labels:
        - "com.example.version=1.0"
    
    image: myapp:${VERSION:-latest}
    
    # Container configuration
    container_name: myapp-web-${ENVIRONMENT:-dev}
    hostname: web-server
    domainname: example.com
    
    # Runtime options
    restart: unless-stopped
    init: true                   # Use init process
    read_only: true             # Read-only filesystem
    
    # User and security
    user: "1001:1001"
    group_add:
      - "staff"
    privileged: false
    cap_add:
      - SYS_TIME
    cap_drop:
      - ALL
    security_opt:
      - no-new-privileges:true
      - seccomp:unconfined
    
    # Resources
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
        order: start-first
      resources:
        limits:
          cpus: '2.0'
          memory: 1G
        reservations:
          cpus: '0.5'
          memory: 512M
    
    # Networking
    ports:
      - target: 3000
        published: 3000
        protocol: tcp
        mode: host
    expose:
      - "3000"
    
    # Health and monitoring
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:3000/health || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 60s
      disable: false
    
    # Logging
    logging:
      driver: "fluentd"
      options:
        fluentd-address: "localhost:24224"
        tag: "myapp.web"
    
    # Process management
    pid: "host"                 # Use host PID namespace
    ipc: "host"                 # Use host IPC namespace
    
    # Device access
    devices:
      - "/dev/ttyUSB0:/dev/ttyUSB0"
    
    # Volumes and mounts
    volumes:
      - type: bind
        source: ./app
        target: /app
        read_only: true
      - type: volume
        source: data
        target: /data
        volume:
          nocopy: true
      - type: tmpfs
        target: /tmp
        tmpfs:
          size: 100M
    
    # Ulimits
    ulimits:
      nproc: 65535
      nofile:
        soft: 20000
        hard: 40000
    
    # System controls
    sysctls:
      - net.core.somaxconn=1024
      - net.ipv4.tcp_syncookies=0
    
    # Labels
    labels:
      - "com.example.service=web"
      - "com.example.version=1.0"
      - "traefik.enable=true"
      - "traefik.http.routers.web.rule=Host(`example.com`)"

  # Database with external configuration
  database:
    image: postgres:14
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init:/docker-entrypoint-initdb.d:ro
    configs:
      - source: postgres_config
        target: /etc/postgresql/postgresql.conf
    secrets:
      - db_root_password
      - db_user_password

# External resources
volumes:
  postgres_data:
    external: true
    name: myapp_postgres_data

networks:
  default:
    external: true
    name: myapp_network

configs:
  postgres_config:
    file: ./config/postgresql.conf

secrets:
  db_root_password:
    file: ./secrets/db_root_password.txt
  db_user_password:
    external: true
```

## Compose Commands

📖 <a href="https://docs.docker.com/compose/reference/" target="_blank">CLI Reference</a>

```bash
# Project management
docker-compose -p myproject up       # Custom project name
docker-compose --file custom.yml up  # Custom compose file
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up

# Service operations
docker-compose up service1 service2  # Start specific services
docker-compose stop service1         # Stop specific service
docker-compose restart service1      # Restart specific service
docker-compose kill service1         # Force stop service

# Watch mode (Compose 2.22+)
docker-compose up --watch            # Enable watch mode for all services
docker-compose watch                 # Start watch mode only
docker-compose alpha watch           # Alpha command (older versions)

# Logs and monitoring
docker-compose logs                  # All service logs
docker-compose logs --tail=50 web   # Last 50 lines from web service
docker-compose logs -f              # Follow all logs
docker-compose logs --since="2023-01-01T00:00:00"

# Execution and debugging
docker-compose exec web bash        # Execute command in running service
docker-compose run --rm web npm test # Run one-off command
docker-compose run --no-deps web sh # Run without dependencies

# Building and images
docker-compose build                 # Build all services
docker-compose build --no-cache web # Build without cache
docker-compose pull                 # Pull latest images
docker-compose push                 # Push images to registry

# Configuration and validation
docker-compose config               # Validate and view merged config
docker-compose config --services   # List services
docker-compose config --volumes    # List volumes
docker-compose config --resolve-image-digests # Show image digests

# Scaling and management
docker-compose up --scale web=3 --scale worker=2
docker-compose top                  # Display running processes
docker-compose port web 3000       # Show public port for service port

# Cleanup
docker-compose down --volumes       # Remove containers and volumes
docker-compose down --rmi all       # Remove containers and images
docker-compose rm -f                # Remove stopped containers

# Environment and overrides
docker-compose --env-file .env.prod up
docker-compose -f docker-compose.yml -f docker-compose.override.yml up

# Convert to Kubernetes
docker-compose config | kompose convert -f -
```

## Watch Configuration

📖 <a href="https://docs.docker.com/compose/file-watch/" target="_blank">Watch Documentation</a>

```yaml
# Watch configuration for development
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    develop:
      watch:
        # Sync files and rebuild
        - action: sync
          path: ./src
          target: /app/src
          ignore:
            - "*.log"
            - "*.tmp"
        
        # Rebuild image on Dockerfile changes
        - action: rebuild
          path: .
          ignore:
            - "node_modules/"
            - ".git/"
        
        # Sync and restart container
        - action: sync+restart
          path: ./config
          target: /app/config

  # Advanced watch configuration
  api:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - ./api:/app/api
      - ./shared:/app/shared
    develop:
      watch:
        # Multiple sync paths
        - action: sync
          path: ./api/src
          target: /app/api/src
        
        - action: sync
          path: ./shared
          target: /app/shared
        
        # Rebuild on package.json changes
        - action: rebuild
          path: ./api/package.json
        
        # Sync and restart on config changes
        - action: sync+restart
          path: ./api/config
          target: /app/api/config
          ignore:
            - "*.example"
            - "*.bak"

  # Database with watch for init scripts
  database:
    image: postgres:14
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: dev
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./db/init:/docker-entrypoint-initdb.d
    develop:
      watch:
        # Restart database when init scripts change
        - action: sync+restart
          path: ./db/init
          target: /docker-entrypoint-initdb.d

  # Frontend with hot reload
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile.dev
    ports:
      - "3001:3000"
    volumes:
      - ./frontend/src:/app/src
      - ./frontend/public:/app/public
    environment:
      - CHOKIDAR_USEPOLLING=true
    develop:
      watch:
        # Sync source files (hot reload handles restart)
        - action: sync
          path: ./frontend/src
          target: /app/src
        
        # Sync public assets
        - action: sync
          path: ./frontend/public
          target: /app/public
        
        # Rebuild on dependency changes
        - action: rebuild
          path: ./frontend/package.json
        
        - action: rebuild
          path: ./frontend/package-lock.json

volumes:
  postgres_data:

# Watch actions explained:
# - sync: Copy files into container
# - rebuild: Rebuild the service image
# - sync+restart: Copy files and restart container

# Usage examples:
# docker-compose up --watch                    # Start with watch enabled
# docker-compose watch                         # Watch mode only
# docker-compose up web --watch                # Watch specific service
```

```bash
# Watch mode commands
docker-compose up --watch                     # Start all services with watch
docker-compose up web --watch                 # Watch specific service only
docker-compose watch                          # Start watch mode without up
docker-compose alpha watch                    # Alpha command (older versions)

# Watch with specific compose files
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --watch

# Watch configuration validation
docker-compose config                         # Verify watch configuration

# Example development workflow with watch
#!/bin/bash
echo "Starting development environment with file watching..."

# Start services with watch mode
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --watch

# The watch configuration will:
# 1. Sync source code changes instantly
# 2. Rebuild images when Dockerfile changes
# 3. Restart containers when config changes
# 4. Ignore specified files and directories
```

```yaml
# Complete development setup with watch
version: '3.8'

services:
  # Node.js app with hot reload
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
      target: development
    ports:
      - "3000:3000"
      - "9229:9229"  # Debug port
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DEBUG=app:*
    develop:
      watch:
        # Sync source files for hot reload
        - action: sync
          path: ./src
          target: /app/src
          ignore:
            - "**/*.test.js"
            - "**/*.spec.js"
        
        # Sync package files and rebuild
        - action: rebuild
          path: ./package.json
        
        - action: rebuild
          path: ./package-lock.json
        
        # Rebuild on Dockerfile changes
        - action: rebuild
          path: ./Dockerfile.dev
        
        # Sync environment files and restart
        - action: sync+restart
          path: ./.env.development
          target: /app/.env.development

  # Python app with auto-reload
  python-api:
    build:
      context: ./api
      dockerfile: Dockerfile.dev
    ports:
      - "8000:8000"
    volumes:
      - ./api:/app
    environment:
      - FLASK_ENV=development
      - FLASK_DEBUG=1
    develop:
      watch:
        # Sync Python files
        - action: sync
          path: ./api/src
          target: /app/src
          ignore:
            - "**/__pycache__"
            - "**/*.pyc"
        
        # Rebuild on requirements changes
        - action: rebuild
          path: ./api/requirements.txt
        
        # Sync config and restart
        - action: sync+restart
          path: ./api/config
          target: /app/config

  # React frontend
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile.dev
    ports:
      - "3001:3000"
    volumes:
      - ./frontend/src:/app/src
      - ./frontend/public:/app/public
    environment:
      - FAST_REFRESH=true
      - CHOKIDAR_USEPOLLING=true
    develop:
      watch:
        # Sync source files (React hot reload handles updates)
        - action: sync
          path: ./frontend/src
          target: /app/src
          ignore:
            - "**/*.test.js"
            - "**/*.test.jsx"
        
        # Sync public assets
        - action: sync
          path: ./frontend/public
          target: /app/public
        
        # Rebuild on package changes
        - action: rebuild
          path: ./frontend/package.json

# Development script with watch
# dev-watch.sh
#!/bin/bash
set -e

echo "🚀 Starting development environment with file watching..."

# Ensure we have the latest images
docker-compose build

# Start with watch mode
docker-compose up --watch

# This will:
# ✅ Start all services
# ✅ Watch for file changes
# ✅ Auto-sync code changes
# ✅ Auto-rebuild on dependency changes
# ✅ Auto-restart on config changes
```

## Docker Registry

📖 <a href="https://docs.docker.com/registry/" target="_blank">Registry Documentation</a>

```bash
# Docker Hub operations
docker login                        # Login to Docker Hub
docker login registry.example.com   # Login to private registry

docker tag myapp:latest username/myapp:latest
docker push username/myapp:latest
docker pull username/myapp:latest

# Private registry setup
docker run -d -p 5000:5000 --name registry registry:2

# Push to private registry
docker tag myapp:latest localhost:5000/myapp:latest
docker push localhost:5000/myapp:latest

# Registry with authentication
docker run -d -p 5000:5000 \
  --name registry \
  -v $(pwd)/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e "REGISTRY_AUTH_HTPASSWD