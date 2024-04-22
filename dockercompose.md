# Working with (Docker)[[docker]] Compose

- using a different yml file (for different environments)

```
docker-compose -f docker-compose.prod.yml up
```

- running in detached mode

```
docker-compose up -d
```

- show logs of detached containers. -f is for follow

```
docker-compose logs -f
```

- exec something inside a container

```
docker-compose exec <service> <command>
```

- rebuild a container

  ```
  docker-compose build --no-cache <service>
  ```

  - for some reason it is required to run this. Otherwise the next command won't work

  ```
  docker-compose down --remove-orphans
  ```

  ```
  docker-compose -f docker-compose.prod.yml up --build -d
  ```
