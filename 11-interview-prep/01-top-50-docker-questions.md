# 01 Top 50 Docker Questions

## Basics
1. What is Docker?
   - Docker is a platform to package and run apps in containers.
2. Difference between image and container?
   - Image is blueprint, container is running instance.
3. What problem does Docker solve?
   - Environment mismatch across machines.
4. Why use tags?
   - Tags track versions and support safe rollbacks.
5. What is Docker Hub?
   - Public registry for sharing images.
6. What does docker run do?
   - Creates and starts a container from an image.
7. What does docker pull do?
   - Downloads image layers from registry.
8. What does docker ps show?
   - Running containers.
9. docker stop vs docker kill?
   - stop is graceful, kill is immediate.
10. What is docker exec?
    - Run command inside running container.

## Images and Dockerfile
11. CMD vs ENTRYPOINT?
    - CMD provides default args; ENTRYPOINT defines fixed executable.
12. COPY vs ADD?
    - COPY is straightforward local copy; ADD also supports URL/tar extraction.
13. What is a Docker layer?
    - Immutable filesystem diff from each instruction.
14. How does caching work?
    - Unchanged steps reuse old layer; changed input invalidates following layers.
15. Why order Dockerfile instructions?
    - Better cache reuse and faster builds.
16. What is multistage build?
    - Build in one stage, run in smaller final stage.
17. How reduce image size?
    - Use slim/alpine base, multistage, remove dev deps.
18. Why avoid latest tag?
    - Non-reproducible builds.
19. What is .dockerignore?
    - Excludes files from build context.
20. What is base image?
    - Starting image used by FROM.

## Containers
21. Container lifecycle states?
    - created, running, paused, exited, removed.
22. What does --rm do?
    - Auto-removes container after it stops.
23. Why use --name?
    - Easier management than random IDs.
24. What is detached mode?
    - Runs in background with -d.
25. How view logs?
    - docker logs --follow <name>.
26. How inspect container details?
    - docker inspect <name>.
27. How copy files in/out?
    - docker cp src dest.
28. How check resource usage?
    - docker stats.
29. Why container exits immediately?
    - Main process finished.
30. How keep container alive for debug?
    - Run shell or long-running process.

## Networking
31. What is bridge networking?
    - Default isolated network on one host.
32. How containers communicate on same network?
    - Use container/service name as hostname.
33. EXPOSE vs port publishing?
    - EXPOSE documents port; publish (-p) maps host port.
34. What is host network mode?
    - Container shares host network stack.
35. none mode use case?
    - Maximum isolation, no external networking.
36. Why custom networks?
    - Better isolation and built-in DNS.

## Storage
37. Named volume vs bind mount?
    - Named volume managed by Docker; bind mount maps host path.
38. Why data disappears sometimes?
    - Stored inside container writable layer instead of volume.
39. How backup a named volume?
    - Run helper container to tar volume data.
40. What is tmpfs mount?
    - In-memory mount cleared on stop.

## Compose and Production
41. What does docker compose down -v do?
    - Stops services and removes containers, network, and named volumes in project.
42. Why Compose?
    - Manage multi-container apps with one file.
43. What is depends_on?
    - Startup dependency order helper.
44. How pass secrets securely?
    - Use secret manager or Docker secrets, not plain env vars.
45. What is Docker Swarm?
    - Built-in clustering and service orchestration.
46. Manager vs worker in Swarm?
    - Managers schedule; workers run tasks.
47. Why health checks?
    - Confirm service readiness and ongoing health.
48. Why set resource limits?
    - Prevent one container from exhausting host.
49. How scan image vulnerabilities?
    - docker scout quickview <image> or trivy image <image>.
50. Pipeline stages with Docker?
    - build, test, push, deploy.

## Example commands
`ash
# Build and tag versioned image
docker build --tag myuser/user-service:1.0.0 .

# Push to registry
docker push myuser/user-service:1.0.0
`

Expected output:
`	ext
Successfully tagged myuser/user-service:1.0.0
The push refers to repository [docker.io/myuser/user-service]
`

## Quick Revision
- Top 50 Docker Questions is easier when you think in small building blocks.
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
