# Sudo related stuff

## Make sudo not ask for password for specific command

```bash
sudo visudo
```

Add the following line to the file:

```bash
<username> ALL=(ALL) NOPASSWD: /path/to/command
```

example:

```bash
your_username ALL=(ALL) NOPASSWD: /bin/systemctl restart steam-api-frontend.service
```


