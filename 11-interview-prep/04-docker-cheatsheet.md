# 04 Docker Cheatsheet

## Image commands
- docker pull IMAGE:TAG : download image.
- docker build --tag NAME:TAG PATH : build image.
- docker image ls : list images.
- docker rmi IMAGE:TAG : remove image.
- docker image inspect IMAGE:TAG : view metadata.
- docker history IMAGE:TAG : view layer history.
- docker push NAME:TAG : push image.

## Container commands
- docker run [flags] IMAGE:TAG : run container.
- docker ps --all : list containers.
- docker stop NAME : graceful stop.
- docker kill NAME : force stop.
- docker rm NAME : remove container.
- docker logs --follow NAME : stream logs.
- docker exec --interactive --tty NAME sh : shell inside container.
- docker stats : resource usage.

## Volume commands
- docker volume create VOLUME : create volume.
- docker volume ls : list volumes.
- docker volume inspect VOLUME : details.
- docker volume rm VOLUME : remove volume.
- docker volume prune --force : remove unused volumes.

## Network commands
- docker network create NET : create network.
- docker network ls : list networks.
- docker network inspect NET : details.
- docker network connect NET CONTAINER : connect container.
- docker network disconnect NET CONTAINER : disconnect container.

## Compose commands
- docker compose up --detach : start stack.
- docker compose down : stop and remove stack.
- docker compose down --volumes : remove stack and volumes.
- docker compose ps : list services.
- docker compose logs --follow : stream logs.
- docker compose exec SERVICE sh : shell inside service.
- docker compose build : build services.
- docker compose pull : pull images.

## Dockerfile instructions quick reference
- FROM, WORKDIR, COPY, ADD, RUN, ENV, EXPOSE, CMD, ENTRYPOINT, ARG, LABEL, USER.

## Most used docker run flags
- --detach, --publish, --volume, --env, --name, --rm, --interactive, --tty.

## docker-compose.yml structure
`yaml
services:
  app:
    image: myapp:1.0.0
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
networks:
  app-net:
volumes:
  app-data:
`

## Quick Revision
- Docker Cheatsheet is easier when you think in small building blocks.
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
