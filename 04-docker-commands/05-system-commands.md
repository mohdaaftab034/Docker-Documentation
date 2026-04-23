# 05 System Commands

## What is it
A practical guide to Docker commands you will run daily.

## Why do we need it
System commands help you inspect environment health and reclaim disk space.

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
### docker info
What it does: Shows engine and system details.

Syntax:
`ash
docker info
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker info --format '{{.ServerVersion}}'
`

Expected output:
`	ext
27.1.1
`
### docker version
What it does: Shows client and server versions.

Syntax:
`ash
docker version
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker version
`

Expected output:
`	ext
Client: ... Server: ...
`
### docker system df
What it does: Shows Docker disk usage.

Syntax:
`ash
docker system df
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker system df --verbose
`

Expected output:
`	ext
Images: ... Containers: ...
`
### docker system prune
What it does: Removes unused resources.

Syntax:
`ash
docker system prune --force
`

Common flags:
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker system prune --all --volumes --force
`

Expected output:
`	ext
Total reclaimed space: ...
`
### docker events
What it does: Streams real-time events from daemon.

Syntax:
`ash
docker events
`

Common flags:
- $_
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker events --filter type=container
`

Expected output:
`	ext
2026-04-23T... container start
`
### docker top
What it does: Shows processes inside container.

Syntax:
`ash
docker top CONTAINER
`

Common flags:
- $_

Real-world example:
`ash
# Explain: use a specific image tag for reproducible runs
docker top user-service
`

Expected output:
`	ext
PID USER TIME COMMAND
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
- [Installation and Setup](../01-introduction/05-installation-and-setup.md)
- [Troubleshooting Guide](../11-interview-prep/03-troubleshooting-guide.md)

## Quick Revision
- 05 System Commands is easier when you think in small building blocks.
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
