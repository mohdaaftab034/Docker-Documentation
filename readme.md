# Docker Notes - Complete Beginner to Practical Guide

This file teaches Docker in simple language.

Goal of this guide:
- Explain what Docker is and why it matters.
- Explain every important concept in easy words.
- Show practical code examples after theory.
- Help you think clearly about how Docker works internally.

## Topic Navigation Buttons

Use these buttons to jump quickly between topics.

<a href="#1-what-is-docker"><button>1. What is Docker?</button></a>
<a href="#2-big-picture-virtual-machine-vs-container"><button>2. VM vs Container</button></a>
<a href="#3-core-docker-terms-must-know"><button>3. Core Terms</button></a>
<a href="#4-how-docker-works-internally-simple-but-accurate"><button>4. Internal Working</button></a>
<a href="#5-install-and-verify-docker"><button>5. Install & Verify</button></a>
<a href="#6-first-practical-commands"><button>6. Practical Commands</button></a>
<a href="#7-important-container-lifecycle-concepts"><button>7. Lifecycle</button></a>
<a href="#8-dockerfile-deep-explanation"><button>8. Dockerfile</button></a>
<a href="#9-image-layers-and-build-cache"><button>9. Layers & Cache</button></a>
<a href="#10-cmd-vs-entrypoint"><button>10. CMD vs ENTRYPOINT</button></a>
<a href="#11-dockerignore-very-important"><button>11. .dockerignore</button></a>
<a href="#12-volumes-and-data-persistence"><button>12. Volumes</button></a>
<a href="#13-docker-networking-simplified"><button>13. Networking</button></a>
<a href="#14-environment-variables-and-secrets"><button>14. Env & Secrets</button></a>
<a href="#15-resource-limits"><button>15. Resource Limits</button></a>
<a href="#16-health-checks"><button>16. Health Checks</button></a>
<a href="#17-multi-stage-builds"><button>17. Multi-Stage Builds</button></a>
<a href="#18-docker-compose-multi-container-apps"><button>18. Docker Compose</button></a>
<a href="#19-push-and-pull-workflow-team-collaboration"><button>19. Push/Pull Workflow</button></a>
<a href="#20-debugging-and-inspection-commands"><button>20. Debugging</button></a>
<a href="#21-security-fundamentals"><button>21. Security</button></a>
<a href="#22-common-mistakes-and-how-to-avoid-them"><button>22. Common Mistakes</button></a>
<a href="#23-docker-prune-and-cleanup"><button>23. Cleanup</button></a>
<a href="#24-end-to-end-example-theory-practice"><button>24. End-to-End Example</button></a>
<a href="#25-docker-compose-example-for-app-database"><button>25. Compose App + DB</button></a>
<a href="#26-build-arguments-vs-environment-variables"><button>26. ARG vs ENV</button></a>
<a href="#27-port-mapping-clarified"><button>27. Port Mapping</button></a>
<a href="#28-logs-stdout-and-best-practice"><button>28. Logs</button></a>
<a href="#29-timezones-and-locale-notes"><button>29. Timezones</button></a>
<a href="#30-docker-in-development-vs-production"><button>30. Dev vs Prod</button></a>
<a href="#31-glossary-quick-definitions"><button>31. Glossary</button></a>
<a href="#32-recommended-learning-path"><button>32. Learning Path</button></a>
<a href="#33-quick-command-cheat-sheet"><button>33. Cheat Sheet</button></a>
<a href="#34-final-mental-model"><button>34. Mental Model</button></a>
<a href="#35-practice-exercises"><button>35. Practice Exercises</button></a>

---

## 1) What is Docker?

Docker is a platform to package an application and everything it needs (code, runtime, libraries, system tools, configuration) into a standard unit called a **container**.

Think of a container like a "portable app box":
- The app behaves the same way on your laptop, test server, and production server.
- You avoid "works on my machine" problems.

### Why Docker was created
Before Docker, teams often had problems like:
- Different OS versions
- Different language versions (Node, Python, Java)
- Missing packages
- Manual setup mistakes

Docker solves this by making environments reproducible.

---

## 2) Big Picture: Virtual Machine vs Container

This concept is very important.

### Virtual Machine (VM)
A VM includes:
- Full guest operating system
- App + dependencies
- Runs on top of a hypervisor

Result:
- Heavier in size
- Slower startup
- Strong isolation

### Container
A container includes:
- App + dependencies
- Shares host OS kernel
- Runs as isolated process

Result:
- Much lighter
- Starts quickly
- Easy to scale

### Simple analogy
- VM = full separate house
- Container = separate apartment in same building

Each apartment is isolated, but all share the building structure.

---

## 3) Core Docker Terms (Must Know)

### 3.1 Docker Engine
Main software that runs containers.

Includes:
- Docker daemon (`dockerd`) - background service doing the actual work
- Docker CLI (`docker`) - command tool you type in terminal
- REST API - internal communication interface

### 3.2 Image
A read-only template used to create containers.

Contains:
- App code
- Runtime (example: Node.js)
- Libraries
- Environment setup

Important:
- Image is like a class blueprint.
- Container is a running object created from that blueprint.

### 3.3 Container
A running (or stopped) instance of an image.

Key points:
- Has its own filesystem layer (writable layer)
- Has isolated process space
- Can be started/stopped/deleted quickly

### 3.4 Dockerfile
A text file with instructions to build an image.

Example instructions:
- `FROM` base image
- `RUN` install packages
- `COPY` project files
- `CMD` default startup command

### 3.5 Registry
A place where images are stored.

Examples:
- Docker Hub
- GitHub Container Registry
- AWS ECR
- Azure Container Registry

### 3.6 Repository and Tag
- Repository: image name collection (example: `nginx`)
- Tag: specific version (example: `nginx:1.27`)

Without tag, Docker uses `latest` by default.

---

## 4) How Docker Works Internally (Simple but Accurate)

When you run:

```bash
docker run nginx
```

Docker does this:
1. Checks if `nginx` image exists locally.
2. If not, pulls it from registry.
3. Creates a new container writable layer.
4. Sets up network interface.
5. Starts process defined by image (`CMD`/`ENTRYPOINT`).
6. Streams logs/output to your terminal.

### Under the hood technologies
Docker uses Linux kernel features:
- **Namespaces**: process isolation (PID, network, mount, etc.)
- **cgroups**: resource control (CPU, memory)
- **Union filesystem**: layered image filesystem

You do not need to master kernel internals now, but knowing these names helps understanding.

---

## 5) Install and Verify Docker

After installing Docker Desktop (Windows/Mac) or Docker Engine (Linux), verify:

```bash
docker --version
docker info
```

Test installation:

```bash
docker run hello-world
```

If successful, Docker downloads a small test image and prints a confirmation message.

---

## 6) First Practical Commands

### Pull an image

```bash
docker pull nginx:latest
```

### List images

```bash
docker images
```

### Run a container

```bash
docker run -d --name my-nginx -p 8080:80 nginx:latest
```

Meaning:
- `-d` -> detached mode (runs in background)
- `--name` -> custom container name
- `-p 8080:80` -> map host port 8080 to container port 80

### See running containers

```bash
docker ps
```

### See all containers (including stopped)

```bash
docker ps -a
```

### View logs

```bash
docker logs my-nginx
```

### Stop and remove

```bash
docker stop my-nginx
docker rm my-nginx
```

---

## 7) Important Container Lifecycle Concepts

Container states:
- Created
- Running
- Paused
- Stopped (Exited)
- Removed

Useful commands:

```bash
docker start <container>
docker stop <container>
docker restart <container>
docker pause <container>
docker unpause <container>
```

### One-time container vs long-running service
- One-time: run command and exit
- Long-running: web server or API service

Example one-time:

```bash
docker run --rm alpine echo "Hello from Alpine"
```

`--rm` automatically removes container after exit.

---

## 8) Dockerfile Deep Explanation

Here is a simple Node.js Dockerfile:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

### What each line means
- `FROM node:20-alpine`
  - Base image to start from.
  - Alpine variant is smaller.

- `WORKDIR /app`
  - Sets working directory inside image/container.

- `COPY package*.json ./`
  - Copies dependency files first.
  - Good for build cache efficiency.

- `RUN npm ci`
  - Executes command at build time.
  - Installs exact dependencies.

- `COPY . .`
  - Copies the rest of project files.

- `EXPOSE 3000`
  - Documents app listens on port 3000.
  - Does not publish port by itself.

- `CMD ["npm", "start"]`
  - Default command when container starts.

### Build image from Dockerfile

```bash
docker build -t my-node-app:1.0 .
```

### Run image as container

```bash
docker run -d --name node-app -p 3000:3000 my-node-app:1.0
```

---

## 9) Image Layers and Build Cache

Every Dockerfile instruction creates a layer (simplified view).

Benefits:
- Faster rebuilds (cache)
- Shared layers across images
- Efficient storage

### Cache rule (important)
If one layer changes, all layers after it rebuild.

That is why this order is good:
1. Copy dependency files
2. Install dependencies
3. Copy source code

If source changes often but dependencies do not, Docker reuses dependency layer.

---

## 10) CMD vs ENTRYPOINT

### CMD
Defines default command/arguments.
Can be overridden easily.

### ENTRYPOINT
Defines fixed executable.
Additional args are appended.

Example:

```dockerfile
ENTRYPOINT ["python", "app.py"]
CMD ["--port", "8000"]
```

Running:

```bash
docker run my-image --port 9000
```

Final executed command becomes:

```bash
python app.py --port 9000
```

---

## 11) .dockerignore (Very Important)

This file excludes unnecessary files from build context.

Why needed:
- Faster builds
- Smaller image
- Better security

Example `.dockerignore`:

```txt
node_modules
.git
.env
coverage
dist
*.log
```

Without this, Docker may upload large or sensitive files during build.

---

## 12) Volumes and Data Persistence

Containers are ephemeral by design.
If container is deleted, writable layer data is lost.

Use volumes to persist data.

### Named volume example

```bash
docker volume create mysql_data
docker run -d --name mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -v mysql_data:/var/lib/mysql \
  mysql:8
```

### Bind mount example

```bash
docker run -d --name app \
  -v ${PWD}:/app \
  node:20-alpine
```

Difference:
- Named volume: managed by Docker
- Bind mount: direct host path mapping

Use bind mounts for development, named volumes for managed persistent data.

---

## 13) Docker Networking Simplified

By default, containers are isolated.
Docker networking allows communication.

Common network types:
- `bridge` (default on single host)
- `host` (shares host network stack)
- `none` (no networking)

### Create custom bridge network

```bash
docker network create app_net
```

### Run two containers in same network

```bash
docker run -d --name db --network app_net postgres:16
docker run -d --name api --network app_net my-api:1.0
```

Now `api` can connect to database using hostname `db`.

---

## 14) Environment Variables and Secrets

### Environment variables
Pass runtime config without changing image.

```bash
docker run -d --name web \
  -e APP_ENV=production \
  -e PORT=8080 \
  my-web-app:1.0
```

### .env file with Docker

```bash
docker run --env-file .env my-web-app:1.0
```

### Important warning
Do not hardcode secrets in Dockerfile:
- API keys
- passwords
- tokens

Use secret managers in production.

---

## 15) Resource Limits

Prevent one container from consuming all system resources.

```bash
docker run -d --name worker \
  --cpus="1.5" \
  --memory="512m" \
  my-worker:1.0
```

This improves system stability in multi-container environments.

---

## 16) Health Checks

Health checks tell if app is actually healthy, not just "running".

Dockerfile example:

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

If check fails repeatedly, container status becomes `unhealthy`.

---

## 17) Multi-Stage Builds

Purpose:
- Keep final image small
- Avoid shipping build tools

Example for Node build:

```dockerfile
# Stage 1: build
FROM node:20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: runtime
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --omit=dev
CMD ["node", "dist/server.js"]
```

Result:
- Build tools remain in first stage only.
- Runtime image is cleaner and smaller.

---

## 18) Docker Compose (Multi-Container Apps)

Docker Compose helps run multiple services together.

Example `docker-compose.yml`:

```yaml
version: "3.9"
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app123
      POSTGRES_DB: appdb
    volumes:
      - db_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  api:
    build: .
    depends_on:
      - db
    environment:
      DATABASE_URL: postgresql://app:app123@db:5432/appdb
    ports:
      - "3000:3000"

volumes:
  db_data:
```

Commands:

```bash
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

Why Compose is useful:
- One file describes entire local stack.
- Easy onboarding for teams.
- Repeatable dev environments.

---

## 19) Push and Pull Workflow (Team Collaboration)

### Tag image

```bash
docker tag my-node-app:1.0 yourdockerhubname/my-node-app:1.0
```

### Login and push

```bash
docker login
docker push yourdockerhubname/my-node-app:1.0
```

### Pull elsewhere

```bash
docker pull yourdockerhubname/my-node-app:1.0
```

This enables sharing exact app versions across environments.

---

## 20) Debugging and Inspection Commands

### Inspect metadata

```bash
docker inspect <container_or_image>
```

### Real-time events

```bash
docker events
```

### Resource usage

```bash
docker stats
```

### Enter running container shell

```bash
docker exec -it my-nginx sh
```

(Use `bash` if available in image.)

### Show image history

```bash
docker history my-node-app:1.0
```

---

## 21) Security Fundamentals

Good practices:
- Use trusted base images.
- Pin versions (`node:20-alpine`, not just `node`).
- Keep images minimal.
- Do not run as root when possible.
- Scan images for vulnerabilities.
- Do not store secrets in image layers.

### Run as non-root user example

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY . .
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
CMD ["node", "server.js"]
```

---

## 22) Common Mistakes and How to Avoid Them

1. Using `latest` everywhere.
- Problem: unpredictable builds.
- Fix: use fixed tags.

2. Large images.
- Problem: slow pull/deploy.
- Fix: use slim/alpine and multi-stage builds.

3. Copying everything too early.
- Problem: bad cache usage.
- Fix: copy dependency manifest first.

4. Storing secrets in Dockerfile.
- Problem: security leak in image history.
- Fix: runtime env/secrets manager.

5. Not cleaning unused objects.
- Problem: disk fills quickly.
- Fix: periodic prune.

```bash
docker system prune -f
```

---

## 23) Docker Prune and Cleanup

Be careful with cleanup commands.

Useful commands:

```bash
docker container prune -f
docker image prune -f
docker volume prune -f
docker network prune -f
docker system prune -a -f
```

`-a` removes all unused images, not just dangling ones.

---

## 24) End-to-End Example (Theory + Practice)

Scenario:
- You have a simple Express API.
- You want reproducible deployment.

### Step 1: App file (`server.js`)

```js
const express = require("express");
const app = express();
const PORT = process.env.PORT || 3000;

app.get("/", (req, res) => {
  res.send("Dockerized Express API is running");
});

app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok" });
});

app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}`);
});
```

### Step 2: `package.json`

```json
{
  "name": "docker-express-demo",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.19.2"
  }
}
```

### Step 3: Dockerfile

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=5s --retries=3 CMD wget -qO- http://localhost:3000/health || exit 1
CMD ["npm", "start"]
```

### Step 4: Build and run

```bash
docker build -t express-demo:1.0 .
docker run -d --name express-demo -p 3000:3000 express-demo:1.0
```

### Step 5: Test

```bash
curl http://localhost:3000/
curl http://localhost:3000/health
```

---

## 25) Docker Compose Example for App + Database

```yaml
version: "3.9"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      DB_HOST: db
      DB_PORT: 5432
      DB_NAME: appdb
      DB_USER: app
      DB_PASS: app123
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app123
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

Start everything:

```bash
docker compose up -d
```

Stop and remove containers:

```bash
docker compose down
```

---

## 26) Build Arguments vs Environment Variables

### ARG
- Available only at build time.
- Not available in running container unless copied into ENV.

```dockerfile
ARG APP_VERSION=1.0
RUN echo "Building version ${APP_VERSION}"
```

Build with custom value:

```bash
docker build --build-arg APP_VERSION=2.0 -t myapp:2.0 .
```

### ENV
- Available during build and runtime (from point declared onward).

```dockerfile
ENV NODE_ENV=production
```

---

## 27) Port Mapping Clarified

`-p hostPort:containerPort`

Example:

```bash
docker run -p 8080:3000 myapp
```

Meaning:
- App listens on 3000 inside container.
- You access it on 8080 on your machine.

Without `-p`, app may run but not be reachable from host browser.

---

## 28) Logs, Stdout, and Best Practice

Containers should write logs to stdout/stderr.

Why:
- Docker can capture logs.
- Easier integration with log systems.
- Avoid writing logs inside container filesystem.

Check logs:

```bash
docker logs -f <container>
```

---

## 29) Timezones and Locale Notes

Some apps need specific timezone/locale.

You can set env variables:

```dockerfile
ENV TZ=UTC
```

Or install timezone data if needed.

Keep images minimal and only install required packages.

---

## 30) Docker in Development vs Production

### Development
- Bind mounts for live code edits
- More debug tools
- Possibly larger images for convenience

### Production
- Immutable image
- Smaller and secure image
- Non-root user
- Health checks
- Resource limits
- Proper restart strategy/orchestration

---

## 31) Glossary (Quick Definitions)

- Image: Read-only template for containers.
- Container: Running instance of an image.
- Dockerfile: Recipe to build image.
- Registry: Remote image storage.
- Volume: Persistent data storage.
- Bind mount: Host directory mounted in container.
- Tag: Version label of image.
- Build context: Files sent to Docker daemon during build.
- Layer: Incremental filesystem change in image.
- Compose: Tool to run multi-container apps.

---

## 32) Recommended Learning Path

1. Learn basic commands (`pull`, `run`, `ps`, `logs`, `exec`).
2. Build your own image with Dockerfile.
3. Understand layers and cache optimization.
4. Learn volumes and networking.
5. Use Docker Compose for multi-service apps.
6. Learn image hardening and security basics.
7. Practice CI/CD pipeline integration with Docker builds.

---

## 33) Quick Command Cheat Sheet

```bash
# Images
docker pull <image>
docker images
docker rmi <image>

# Containers
docker run <image>
docker ps
docker ps -a
docker stop <container>
docker rm <container>
docker logs -f <container>
docker exec -it <container> sh

# Build
docker build -t myapp:1.0 .

# Volumes and networks
docker volume ls
docker network ls

# Compose
docker compose up -d
docker compose down
docker compose logs -f
```

---

## 34) Final Mental Model

Keep this mental model:
- Dockerfile creates image.
- Image starts container.
- Container is isolated process with writable layer.
- Volumes store data permanently.
- Networks enable container communication.
- Compose manages multiple containers as one application stack.

If you understand this model deeply, most Docker tasks become straightforward.

---

## 35) Practice Exercises

1. Create a Dockerfile for a simple Python Flask app.
2. Add `.dockerignore` and compare build speed.
3. Add health check endpoint and Docker healthcheck.
4. Use named volume with PostgreSQL and verify data persists.
5. Create Compose file for app + db + redis.
6. Rebuild after changing only app code and observe cache behavior.
7. Create a non-root Docker image and test permissions.

---

You can treat this file as your Docker notebook. Expand each section with your own experiments and command outputs as you practice.
