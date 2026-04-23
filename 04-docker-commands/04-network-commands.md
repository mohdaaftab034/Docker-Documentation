# 04 Network Commands

## What is it
A practical guide to Docker commands you will run daily.

## Why do we need it
Network commands connect services safely and predictably.

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
### docker network create
What it does: Creates a network.

Syntax:
`ash
docker network create NETWORK_NAME
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network create app-net
`

Expected output:
`	ext
app-net
`
### docker network ls
What it does: Lists networks.

Syntax:
`ash
docker network ls
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network ls --filter name=app
`

Expected output:
`	ext
NETWORK ID NAME DRIVER
`
### docker network inspect
What it does: Shows network details and connected containers.

Syntax:
`ash
docker network inspect NETWORK_NAME
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network inspect app-net
`

Expected output:
`	ext
Containers: { ... }
`
### docker network connect
What it does: Connects container to network.

Syntax:
`ash
docker network connect NETWORK_NAME CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network connect app-net user-service
`

Expected output:
`	ext
(no output on success)
`
### docker network disconnect
What it does: Disconnects container from network.

Syntax:
`ash
docker network disconnect NETWORK_NAME CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network disconnect app-net user-service
`

Expected output:
`	ext
(no output on success)
`
### docker network rm
What it does: Removes unused network.

Syntax:
`ash
docker network rm NETWORK_NAME
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network rm old-net
`

Expected output:
`	ext
old-net
`
### docker network prune
What it does: Removes unused networks.

Syntax:
`ash
docker network prune --force
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker network prune --force
`

Expected output:
`	ext
Deleted Networks: ...
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
- - [Networks](../02-core-concepts/05-networks.md)
- - [Custom Networks](../07-networking/04-custom-networks.md)

## Quick Revision
- 04 Network Commands is easier when you think in small building blocks.
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
