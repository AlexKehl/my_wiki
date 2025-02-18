# Working with [Docker](/docker.md) Compose

- using a different yml file (for different environments)

```
docker compose -f docker-compose.prod.yml up
```

- running in detached mode

```
docker compose up -d
```

- show logs of detached containers. -f is for follow

```
docker compose logs -f
```

- exec something inside a container

```
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

  ```
  docker compose build --no-cache <service>
  ```

  - for some reason it is required to run this. Otherwise the next command won't work

  ```
  docker compose down --remove-orphans
  ```

  ```
  docker compose -f docker-compose.prod.yml up --build -d
  ```
