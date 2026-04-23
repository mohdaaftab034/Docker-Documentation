# 05 Data Persistence Patterns

This part trips everyone up at first, do not worry. We will go slow, and we will connect every idea to real work.

## What is it
Persistence patterns are repeatable storage strategies for common app needs.

## Why do we need it
Patterns help teams avoid ad-hoc storage mistakes.

In real teams, this removes confusion during onboarding, testing, and release. Instead of hearing "it works on my machine," we use the same setup for everyone.

## Real life analogy
Choose the right container for each ingredient in a kitchen.

Think of a shipping company. The box shape is always standard, but the items inside can be totally different. Docker follows that same idea for software.

## How does it work
- Prepare a clean working folder and decide the exact image tag.
- Run commands with explicit names, ports, and flags.
- Check running state using docker ps and inspect where needed.
- Read logs and outputs to confirm behavior.
- Clean unused resources safely after verification.

### Required points for this chapter
- db named volume.
- dev bind mount.
- uploads volume.
- tmpfs secrets.

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
- Using latest tags and getting unexpected changes.
- Skipping inspect commands and debugging blindly.
- Forgetting to map ports or mount storage.
- Not naming containers and losing track during cleanup.

## Best Practices
- Use specific tags like node:18.20.4-alpine3.20.
- Use clear names like user-service and postgres-db.
- Write commands in scripts for repeatability.
- Keep a short checklist for run, verify, cleanup.

## When to use it
Use this whenever you want predictable local development and easy transition to staging and production.

## Related concepts
- [Images](../02-core-concepts/01-images.md)
- [Containers](../02-core-concepts/02-containers.md)
- [Dockerfile](../02-core-concepts/03-dockerfile.md)
- [Docker Compose](../05-docker-compose/01-what-is-docker-compose.md)

## Quick Revision
- 05 Data Persistence Patterns is easier when you think in small building blocks.
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
