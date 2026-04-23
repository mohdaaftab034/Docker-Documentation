# 03 Volume Commands

## What is it
A practical guide to Docker commands you will run daily.

## Why do we need it
Volume commands protect important state like database data and user uploads.

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
### docker volume create
What it does: Creates a named volume.

Syntax:
`ash
docker volume create VOLUME_NAME
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker volume create postgres-db-data
`

Expected output:
`	ext
postgres-db-data
`
### docker volume ls
What it does: Lists volumes.

Syntax:
`ash
docker volume ls
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker volume ls --filter name=postgres
`

Expected output:
`	ext
DRIVER VOLUME NAME
`
### docker volume inspect
What it does: Shows volume details.

Syntax:
`ash
docker volume inspect VOLUME_NAME
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker volume inspect postgres-db-data
`

Expected output:
`	ext
Mountpoint: /var/lib/docker/volumes/...
`
### docker volume rm
What it does: Removes a volume not in use.

Syntax:
`ash
docker volume rm VOLUME_NAME
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker volume rm old-data
`

Expected output:
`	ext
old-data
`
### docker volume prune
What it does: Removes unused volumes.

Syntax:
`ash
docker volume prune --force
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker volume prune --force
`

Expected output:
`	ext
Deleted Volumes: ...
`
### Mount with docker run -v
What it does: Attaches volume to a path in container.

Syntax:
`ash
docker run --volume VOLUME_NAME:/path IMAGE:TAG
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker run --name postgres-db --detach --volume postgres-db-data:/var/lib/postgresql/data postgres:16.4
`

Expected output:
`	ext
Container started
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
- - [Volumes](../02-core-concepts/04-volumes.md)
- - [Data Persistence Patterns](../08-storage/05-data-persistence-patterns.md)

## Quick Revision
- 03 Volume Commands is easier when you think in small building blocks.
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
