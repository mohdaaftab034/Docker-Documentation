# 05 MongoDB with Docker

You just containerized your first app, that is huge. In this chapter, we build a realistic setup from zero.

## What is it
We are running MongoDB with persistent storage and container-based access.

## Why do we need it
Mongo setup can vary by host package manager and local settings.

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
FROM mongo:7.0.11
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
  mongo-db:
    image: mongo:7.0.11
    container_name: mongo-db
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
volumes:
  mongo-data:
`

### Step-by-step commands
`ash
# Start MongoDB service
docker compose up --detach

# Open mongosh inside container
docker exec --interactive --tty mongo-db mongosh

# Example query
# use userdb
# db.users.insertOne({ name: "Ava", role: "admin" })
# db.users.find()
`

Expected terminal output:
`	ext
[+] Running 3/3
 âœ” Network app-net      Created
 âœ” Volume app-data      Created
 âœ” Container myapp-api  Started
`

## How to verify it is working
Run insert and find queries, then restart container and verify data persistence.

## Common issues and fixes
If Node app cannot connect, ensure it uses hostname mongo-db not localhost inside network.

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
- [Networks](../02-core-concepts/05-networks.md)
- [Named Volumes](../08-storage/02-named-volumes.md)

## Quick Revision
- 05 MongoDB with Docker is easier when you think in small building blocks.
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
