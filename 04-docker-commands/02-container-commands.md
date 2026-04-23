# 02 Container Commands

## What is it
A practical guide to Docker commands you will run daily.

## Why do we need it
Container commands are your daily control panel for running apps.

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
### docker run
What it does: Creates and starts a container.

Syntax:
`ash
docker run [FLAGS] IMAGE[:TAG] [COMMAND]
`

Common flags:
- $_
- $_
- $_
- $_
- $_
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker run --name user-service --detach --publish 3000:3000 --env NODE_ENV=production node:18.20.4-alpine3.20
`

Expected output:
`	ext
a1b2c3d4e5f6
`
### docker ps / docker ps -a
What it does: Lists running or all containers.

Syntax:
`ash
docker ps --all
`

Common flags:
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker ps --all --filter name=user-service
`

Expected output:
`	ext
CONTAINER ID   NAMES   STATUS
`
### docker stop and docker kill
What it does: Stops a container gracefully or forcefully.

Syntax:
`ash
docker stop CONTAINER ; docker kill CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker stop user-service
`

Expected output:
`	ext
user-service
`
### docker start and docker restart
What it does: Starts a stopped container or restarts one.

Syntax:
`ash
docker start CONTAINER ; docker restart CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker restart user-service
`

Expected output:
`	ext
user-service
`
### docker rm
What it does: Removes container.

Syntax:
`ash
docker rm CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker rm user-service
`

Expected output:
`	ext
user-service
`
### docker exec -it
What it does: Runs command inside running container.

Syntax:
`ash
docker exec --interactive --tty CONTAINER bash
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker exec --interactive --tty user-service sh
`

Expected output:
`	ext
/app #
`
### docker logs
What it does: Shows container logs.

Syntax:
`ash
docker logs [FLAGS] CONTAINER
`

Common flags:
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker logs --follow --tail 50 user-service
`

Expected output:
`	ext
Server listening on port 3000
`
### docker inspect
What it does: Shows low-level container details.

Syntax:
`ash
docker inspect CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker inspect user-service --format '{{.State.Status}}'
`

Expected output:
`	ext
running
`
### docker cp
What it does: Copies files to or from container.

Syntax:
`ash
docker cp SRC_PATH DEST_PATH
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker cp user-service:/app/logs ./logs
`

Expected output:
`	ext
(no output on success)
`
### docker stats
What it does: Shows live CPU and memory usage.

Syntax:
`ash
docker stats [CONTAINER]
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker stats --no-stream user-service
`

Expected output:
`	ext
CONTAINER CPU % MEM USAGE / LIMIT
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
- - [Containers](../02-core-concepts/02-containers.md)
- - [Networking](../07-networking/05-container-communication.md)

## Quick Revision
- 02 Container Commands is easier when you think in small building blocks.
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
