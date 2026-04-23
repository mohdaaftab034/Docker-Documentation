# 06 Multi Container App

This part trips everyone up at first, do not worry. We will go slow, and we will connect every idea to real work.

## What is it
This chapter builds a Node.js API with MongoDB and Redis using Docker Compose.

## Why do we need it
Real applications need multiple services that start in the right order and discover each other.

In real teams, this removes confusion during onboarding, testing, and release. Instead of hearing "it works on my machine," we use the same setup for everyone.

## Real life analogy
Like a restaurant where kitchen, storage, and cashier work together under one manager.

Think of a shipping company. The box shape is always standard, but the items inside can be totally different. Docker follows that same idea for software.

## How does it work
- Define each service with clear name and image tag.
- Attach services to same default Compose network.
- Use service names like mongo-db and cache-db as hostnames.
- Add health checks so API starts only when dependencies are ready.
- Run stack and test endpoints.

### Required points for this chapter
- Full docker-compose with Node.js, MongoDB, Redis.
- Line by line explanation.
- Service discovery by service name.
- Health checks.

`mermaid
flowchart LR
    Dev[Developer Laptop] --> CLI[Docker CLI]
    CLI --> Engine[Docker Engine]
    Engine --> Image[Image Layers]
    Engine --> Container[Running Container]
    Container --> Volume[Persistent Volume]
    Container --> Network[Docker Network]
`

## Code or Command Example
### WRONG way first
`ash
# WRONG: running with unclear names and no version tag
# This creates confusion when multiple containers are running.
docker run myapp
`

### CORRECT way
`ash
# Pull a specific version so every machine gets the same image
docker pull nginx:1.27.0

# Run with a clear container name and explicit port mapping
docker run --name myapp-web --publish 8080:80 --detach nginx:1.27.0

# List running containers with useful columns
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}"
`

Expected terminal output:
`	ext
1.27.0: Pulling from library/nginx
Digest: sha256:...
Status: Downloaded newer image for nginx:1.27.0

CONTAINER ID   NAMES       IMAGE          PORTS                  STATUS
a1b2c3d4e5f6   myapp-web   nginx:1.27.0   0.0.0.0:8080->80/tcp   Up 5 seconds
`

## Common Mistakes
- Trying localhost inside a container to reach other containers.
- Missing health checks causing startup race conditions.
- Using latest tags and getting random version changes.

## Best Practices
- Use specific tags.
- Use named volumes for database.
- Use depends_on with service_healthy.
- Keep env values in .env file.

## When to use it
Use Compose whenever you run two or more related services in local or staging environments.

## Related concepts
- [Compose File Structure](02-compose-file-structure.md)
- [Container Communication](../07-networking/05-container-communication.md)

## Quick Revision
- 06 Multi Container App is easier when you think in small building blocks.
- We use specific versions and clear names to avoid surprises.
- We test commands step by step and read outputs carefully.
- We prefer safe defaults: least privilege, small images, persistent data paths.
- Practice this file commands once, then repeat without looking.

## Interview Questions
1. What is the main purpose of 
   - It solves repeatability and clarity so teams can run the same app the same way.
2. What beginner mistake is most common in 
   - Skipping basics like tags, names, and ports, then guessing when things fail.
3. How do you verify your setup works?
   - Run inspect and logs commands, then test with a real request.
4. When should you avoid this approach?
   - Avoid it when a simpler option already solves your problem.
## Full docker-compose.yml
`yaml
version: "3.9"
services:
  user-service:
    image: node:18.20.4-alpine3.20
    container_name: user-service
    working_dir: /app
    command: ["sh", "-c", "npm install && npm run dev"]
    volumes:
      - ./api:/app
    environment:
      - PORT=3000
      - MONGO_URL=mongodb://mongo-db:27017/users
      - REDIS_URL=redis://cache-db:6379
    ports:
      - "3000:3000"
    depends_on:
      mongo-db:
        condition: service_healthy
      cache-db:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "http://localhost:3000/health"]
      interval: 10s
      timeout: 3s
      retries: 5
      start_period: 10s

  mongo-db:
    image: mongo:7.0.11
    container_name: mongo-db
    volumes:
      - mongo-data:/data/db
    healthcheck:
      test: ["CMD", "mongosh", "--quiet", "--eval", "db.adminCommand('ping').ok"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 20s

  cache-db:
    image: redis:7.4.0-alpine
    container_name: cache-db
    command: ["redis-server", "--save", "", "--appendonly", "no"]
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 5

volumes:
  mongo-data:
`

