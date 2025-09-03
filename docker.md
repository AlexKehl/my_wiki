# Docker

## List all containers and only theirs ids

```bash
docker ps -q
```

## Copy file from container to host

```bash
docker cp <container_id>:/app/logs/info-2024-05-27.log .
```

## Clean up images/containers/volumes

```bash
docker volume prune
docker image prune
docker container prune
```

## Env specification in Dockerfile

```Dockerfile
ENV NODE_ENV=production
```

## Handling of SIGINT and SIGTERM

Important to run CMD like 

```Dockerfile
CMD ["node_modules/.bin/tsx", "src/server.ts"]
```

- Use node_modules/.bin/tsx to run your app and not tsx directly
because it will not handle SIGINT and SIGTERM signals properly. 
(Executes with sh -c which does not pass signals to the child process)
- Don't use CMD "..." for the same reason

## Clean up space

The prune command will remove all dangling images, containers, networks, and volumes.

```bash
docker <volume|image|container|network> prune
```

Also one can remove the build cache, which can take up a lot of space, with

```bash
docker builder prune
```

- Remove all dangling stuff
```bash
docker system prune -a
```
