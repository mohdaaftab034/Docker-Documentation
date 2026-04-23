# 04 Network Commands

## What is it
Network commands manage how containers communicate with each other and with the outside world.

## Why do we need it
By default, containers are isolated. Networking commands connect services safely and predictably.

## Real life analogy
Networks are like roads between buildings. Without roads, the buildings exist but cannot exchange anything.

## How does it work
- Create a network.
- Attach containers to that network.
- Inspect routes and connected endpoints.
- Disconnect and remove when no longer needed.

```mermaid
flowchart LR
  A[Create Network] --> B[Connect Containers]
  B --> C[Service to Service Calls]
  C --> D[Inspect]
  D --> E[Disconnect and Cleanup]
```

## Code or Command Example
### WRONG way first
```bash
# WRONG: assuming containers can always reach each other by name on default bridge
docker run --name user-service --detach node:18.20.4-alpine3.20
docker run --name billing-service --detach node:18.20.4-alpine3.20
```

### CORRECT way
```bash
# CORRECT: create a custom network and place both containers on it
docker network create app-net
docker run --name user-service --detach --network app-net node:18.20.4-alpine3.20
docker run --name billing-service --detach --network app-net node:18.20.4-alpine3.20
```

Expected terminal output:
```text
app-net
<container-id>
<container-id>
```

## Command Reference

### docker network create
What the command does in one line: Create a new Docker network.

Full syntax:
```bash
docker network create [OPTIONS] NETWORK
```

Common flags:
- --driver: Set driver, for example bridge.
- --subnet: Set custom subnet.
- --gateway: Set network gateway.

Real world example:
```bash
# Create app network with bridge driver
docker network create --driver bridge app-net
```

Expected output:
```text
app-net
```

### docker network ls
What the command does in one line: List Docker networks.

Full syntax:
```bash
docker network ls [OPTIONS]
```

Common flags:
- --filter: Filter by driver or name.
- --format: Custom output formatting.

Real world example:
```bash
# Show only bridge networks
docker network ls --filter driver=bridge
```

Expected output:
```text
NETWORK ID     NAME      DRIVER
abc123def456   app-net   bridge
```

### docker network inspect
What the command does in one line: Show detailed network information.

Full syntax:
```bash
docker network inspect [OPTIONS] NETWORK [NETWORK...]
```

Common flags:
- -f, --format: Print specific fields.

Real world example:
```bash
# Inspect connected containers
docker network inspect app-net
```

Expected output:
```text
"Containers": {
  "user-service": {...},
  "billing-service": {...}
}
```

### docker network connect
What the command does in one line: Connect an existing container to a network.

Full syntax:
```bash
docker network connect [OPTIONS] NETWORK CONTAINER
```

Common flags:
- --alias: Add extra DNS alias inside network.

Real world example:
```bash
# Attach existing container to app network
docker network connect app-net user-service
```

Expected output:
```text
# No output on success
```

### docker network disconnect
What the command does in one line: Disconnect container from a network.

Full syntax:
```bash
docker network disconnect [OPTIONS] NETWORK CONTAINER
```

Common flags:
- -f, --force: Force disconnect.

Real world example:
```bash
# Remove container from app network
docker network disconnect app-net user-service
```

Expected output:
```text
# No output on success
```

### docker network rm
What the command does in one line: Remove one or more unused networks.

Full syntax:
```bash
docker network rm NETWORK [NETWORK...]
```

Common flags:
- No major flags for basic usage.

Real world example:
```bash
# Remove network after containers are detached
docker network rm app-net
```

Expected output:
```text
app-net
```

### docker network prune
What the command does in one line: Remove all unused networks.

Full syntax:
```bash
docker network prune [OPTIONS]
```

Common flags:
- -f, --force: Skip confirmation prompt.
- --filter: Apply filters.

Real world example:
```bash
# Clean unused networks
docker network prune --force
```

Expected output:
```text
Deleted Networks:
...
```

## Common Mistakes
- Assuming localhost inside one container means host or another container.
- Forgetting to place related containers on same network.
- Deleting networks while active containers still use them.

## Best Practices
- Use custom bridge network for app services.
- Use service names as hostnames on same network.
- Inspect network when debugging connection issues.

## When to use it
Use network commands whenever two or more containers must communicate.

## Related concepts
- [Networks](../02-core-concepts/05-networks.md)
- [Custom Networks](../07-networking/04-custom-networks.md)

## Quick Revision
- Networks connect isolated containers.
- Custom bridge network is best for local multi-service apps.
- Inspect reveals IP, aliases, and connected containers.
- Connect and disconnect are dynamic operations.
- Clean old networks with prune carefully.

## Interview Questions
1. Why is custom bridge better than default bridge for most apps?
   - It gives better isolation and automatic DNS by container name.
2. Can two containers communicate without same network?
   - Usually no, they need shared network or published host ports.
3. What does docker network inspect help with?
   - It helps debug connectivity, IP assignment, and attached containers.
4. When do we use docker network connect?
   - When we need to attach an existing container to another network.
5. What is a common localhost mistake?
   - localhost inside container points to that container, not host or other services.
