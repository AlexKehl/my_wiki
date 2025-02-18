# Colima 

Colima is a replacement for [docker](/docker.md) desktop.

## Start
The default VM created by Colima has 2 CPUs, 2GiB memory and 60GiB storage.
But some images like [elastic](/elasticsearch.md) need more memory. So it makes sense
to increase the memory size on startup.

```bash
colima start --cpu 4 --memory 8
```

## docker-compose
To make [docker compose](/dockercompose.md) commands work – this is needed

```bash
mkdir -p ~/.docker/cli-plugins
ln -sfn /opt/homebrew/opt/docker-compose/bin/docker-compose ~/.docker/cli-plugins/docker-compose
```

Also need to remove docker config from docker desktop. Else it will say:
error getting credentials - err: exec: "docker-credential-desktop": executable file not found in $PATH, out: ``
```
rm -rf ~/.docker/config.json
```

After that need to restart colima with 

```bash
colima restart
```

