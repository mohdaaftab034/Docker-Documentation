# 07 Fullstack App

You just containerized your first app, that is huge. In this chapter, we build a realistic setup from zero.

## What is it
We are running React frontend, Node backend, MongoDB, and Nginx together.

## Why do we need it
This mirrors common production architecture and teaches service boundaries.

Without Docker, machine differences break onboarding and release speed. With Docker, we package app code, runtime, dependencies, and startup command in one repeatable unit.

## Real life analogy
Think of a restaurant kitchen chain. Every branch follows the same recipe card, ingredient list, and oven settings. That is what a Dockerfile plus Compose gives us.

## How does it work
- Write app code and dependency file.
- Create a Dockerfile with explicit base image tag.
- Add .dockerignore to avoid sending extra files.
- Build image and run container.
- Add Compose when multiple services are needed.
- Verify by checking logs, health, and API response.

`mermaid
flowchart LR
    Code[Source Code] --> Dockerfile[Dockerfile]
    Dockerfile --> Build[Docker Build]
    Build --> Image[Versioned Image]
    Image --> Run[Container Runtime]
    Run --> Verify[Logs + HTTP Test]
`

## Code or Command Example
### WRONG way first
`ash
# WRONG: using latest and missing clear container name
docker build -t myapp:latest .
docker run -p 3000:3000 myapp:latest
`

### CORRECT way
`ash
# Build with a specific tag for reproducible results
docker build --tag myapp:1.0.0 .

# Run with a clear name and explicit restart behavior
docker run --name myapp-api --publish 3000:3000 --detach --restart unless-stopped myapp:1.0.0

# Confirm container is healthy and running
docker ps --filter name=myapp-api
`

Expected terminal output:
`	ext
[+] Building 18.2s (12/12) FINISHED
Successfully tagged myapp:1.0.0
CONTAINER ID   NAMES      IMAGE        STATUS
f1e2d3c4b5a6   myapp-api  myapp:1.0.0  Up 6 seconds
`

### Dockerfile
`dockerfile
# Example backend Dockerfile
FROM node:18.20.4-alpine3.20
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .
EXPOSE 4000
CMD ["node", "server.js"]
`

### .dockerignore
`gitignore
# Node and package manager folders should not enter build context
node_modules

# Local environment files can contain secrets
.env

# Git metadata is not needed in runtime image
.git

# Build output from local machine should not be copied
dist
coverage
`

### docker-compose.yml
`yaml
version: "3.9"
services:
  frontend:
    image: react-web:1.0.0
    build: ./frontend
    networks:
      - app-net

  backend:
    image: user-service:1.0.0
    build: ./backend
    environment:
      - MONGO_URL=mongodb://mongo-db:27017/fullstack
      - NODE_ENV=production
    depends_on:
      mongo-db:
        condition: service_healthy
    networks:
      - app-net

  mongo-db:
    image: mongo:7.0.11
    volumes:
      - mongo-data:/data/db
    healthcheck:
      test: ["CMD", "mongosh", "--quiet", "--eval", "db.adminCommand('ping').ok"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - app-net

  reverse-proxy:
    image: nginx:1.27.0-alpine
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
    ports:
      - "8080:80"
    depends_on:
      - frontend
      - backend
    networks:
      - app-net

networks:
  app-net:

volumes:
  mongo-data:
`

### Step-by-step commands
`ash
# Clone and enter project
# git clone <repo-url>
# cd project

# Build and run full stack
docker compose up --build --detach

# Check services
docker compose ps

# Test app
curl http://localhost:8080/api/health
`

Expected terminal output:
`	ext
[+] Running 3/3
 âœ” Network app-net      Created
 âœ” Volume app-data      Created
 âœ” Container myapp-api  Started
`

## How to verify it is working
Open app in browser, test API health route, and inspect Mongo data persistence.

## Common issues and fixes
If frontend cannot call backend, verify Nginx route rules and backend hostname.

## Common Mistakes
- Building from the wrong folder so the Dockerfile cannot find files.
- Forgetting to expose or publish the app port.
- Mixing development and production dependencies in one image.

## Best Practices
- Use multistage builds for production images.
- Run as non-root user where possible.
- Keep image tags and compose files versioned in git.

## When to use it
Use this setup when your team wants one clear, repeatable start command for local dev and deployment.

## Related concepts
- [Multi Container App](../05-docker-compose/06-multi-container-app.md)
- [Production Best Practices](../09-docker-in-production/01-production-best-practices.md)

## Quick Revision
- 07 Fullstack App is easier when you think in small building blocks.
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
