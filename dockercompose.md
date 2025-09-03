# Working with [Docker](/docker.md) Compose

## Port mapping

In Docker Compose port mapping:
- `3007:3007` means:
  - Left side (3007) is the exposed/host port
  - Right side (3007) is the internal container port

Since your service runs internally on port 3006 but should be exposed as 3007, you should change it to:
```yaml
ports:
  - '3007:3006'
```

```bash
docker compose -f docker-compose.prod.yml up
```

- running in detached mode

```bash
docker compose up -d
```

- show logs of detached containers. -f is for follow

```bash
docker compose logs -f
```

- exec something inside a container

```bash
docker compose exec <service> <command>
```

- exec inside container via ssh
  - the -T option is required to disable pseudo-tty allocation
    Otherwise there will be an error "the input device is not a TTY" because
    docker expects a TTY (a terminal) but doesn't get one. To fix this issue, you
    need to run the Docker command without trying to allocate a pseudo-TTY. You
    can achieve this by adding the -T option to your docker-compose exec command.

```
ssh alex@europe-hetzner-ai-prod.apprologic.com "cd gptpdf && docker compose exec -T nextjs cat logs/info-2024-05-15.log"
```

- rebuild a container

  ```bash
  docker compose build --no-cache <service>
  ```

  - for some reason it is required to run this. Otherwise the next command won't work

  ```bash
  docker compose down --remove-orphans
  ```

  ```bash
  docker compose -f docker-compose.prod.yml up --build -d
  ```

- run tests


```bash
 docker compose run <container> npm run test
 dc -f docker-compose.dev.yml run api npm run test
 ```
