# 03 React App

You just containerized your first app, that is huge. In this chapter, we build a realistic setup from zero.

## What is it
We are building a React app in Docker and serving static files with Nginx.

## Why do we need it
Frontend build tools differ by machine and produce inconsistent output if not standardized.

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
FROM node:20.14.0-alpine3.20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:1.27.0-alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
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
    build: .
    image: react-web:1.0.0
    ports:
      - "8080:80"
`

### Step-by-step commands
`ash
# Build React production image
docker build --tag react-web:1.0.0 .

# Run web container
docker run --name react-web --publish 8080:80 --detach react-web:1.0.0

# Open in browser
# http://localhost:8080
`

Expected terminal output:
`	ext
[+] Running 3/3
 âœ” Network app-net      Created
 âœ” Volume app-data      Created
 âœ” Container myapp-api  Started
`

## How to verify it is working
Open localhost:8080 and refresh nested routes to confirm SPA routing works.

## Common issues and fixes
If refreshing route returns 404, fix nginx.conf with try_files rule for SPA.

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
- [Multistage Builds](../03-dockerfile-deep-dive/04-multistage-builds.md)
- [Nginx Reverse Proxy](06-nginx-reverse-proxy.md)

## Quick Revision
- 03 React App is easier when you think in small building blocks.
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
