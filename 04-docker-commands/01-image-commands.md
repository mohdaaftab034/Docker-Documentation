# 01 Image Commands

## What is it
A practical guide to Docker commands you will run daily.

## Why do we need it
Image commands help you fetch, inspect, tag, and publish application blueprints.

## Real life analogy
Commands are like buttons on a machine. If you press the right button in the right order, work is smooth.

## How does it work
- We start from inspection commands.
- Then we run action commands.
- We verify outputs after each step.
- We clean up unused resources.

`mermaid
flowchart TD
    A[Inspect] --> B[Create]
    B --> C[Run]
    C --> D[Verify]
    D --> E[Clean Up]
`

## Code or Command Example
### WRONG way first
`ash
# WRONG: deleting resources without checking what they are
docker system prune --force
`

### CORRECT way
`ash
# Check first, then prune carefully
docker system df

# Remove only dangling images first
docker image prune --force
`

Expected terminal output:
`	ext
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          8         4         2.3GB     1.1GB (47%)
Deleted Images:
untagged: <none>:<none>
`

## Command Reference
### docker pull
What it does: Downloads an image from a registry.

Syntax:
`ash
docker pull IMAGE:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker pull node:18.20.4-alpine3.20
`

Expected output:
`	ext
18.20.4-alpine3.20: Pulling from library/node
`
### docker build
What it does: Builds an image from a Dockerfile.

Syntax:
`ash
docker build --tag NAME:TAG PATH
`

Common flags:
- $_
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker build --tag myapp:1.0.0 .
`

Expected output:
`	ext
Successfully tagged myapp:1.0.0
`
### docker images / docker image ls
What it does: Lists local images.

Syntax:
`ash
docker image ls
`

Common flags:
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker image ls --filter reference='node*'
`

Expected output:
`	ext
REPOSITORY   TAG   IMAGE ID   CREATED
`
### docker rmi
What it does: Removes one or more images.

Syntax:
`ash
docker rmi IMAGE:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker rmi myapp:old
`

Expected output:
`	ext
Untagged: myapp:old
`
### docker image prune
What it does: Removes dangling images.

Syntax:
`ash
docker image prune --force
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker image prune --force
`

Expected output:
`	ext
Deleted Images: ...
`
### docker tag
What it does: Creates another tag pointing to same image.

Syntax:
`ash
docker tag SOURCE_IMAGE:TAG TARGET_IMAGE:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker tag myapp:1.0.0 myuser/myapp:1.0.0
`

Expected output:
`	ext
(no output on success)
`
### docker push
What it does: Uploads an image tag to registry.

Syntax:
`ash
docker push NAME:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker push myuser/myapp:1.0.0
`

Expected output:
`	ext
latest: digest: sha256:...
`
### docker image inspect
What it does: Shows image metadata in JSON.

Syntax:
`ash
docker image inspect IMAGE:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker image inspect node:18.20.4-alpine3.20 --format '{{.Os}}/{{.Architecture}}'
`

Expected output:
`	ext
linux/amd64
`
### docker history
What it does: Shows image layer history.

Syntax:
`ash
docker history IMAGE:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker history myapp:1.0.0
`

Expected output:
`	ext
IMAGE   CREATED BY   SIZE
`

## Common Mistakes
- Running destructive commands before listing current resources.
- Using old command syntax from random blog posts.
- Ignoring command output and missing warning lines.

## Best Practices
- Use explicit names and tags.
- Inspect before remove.
- Keep a cleanup routine in local development.

## When to use it
Use these commands every day in development, testing, and debugging.

## Related concepts
- - [Images](../02-core-concepts/01-images.md)
- - [Docker Registry](../02-core-concepts/06-docker-registry.md)

## Quick Revision
- 01 Image Commands is easier when you think in small building blocks.
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
