# Systemd

## View logs

```bash
sudo journalctl --unit=<service-name>
```

## Create new service

1. Create a systemd service file:

```bash
sudo vim /etc/systemd/system/service-name.service
```

2. Add this configuration:

```ini
[Unit]
Description=some description
After=network.target

[Service]
Type=simple
User=your_username
WorkingDirectory=/path/to/your/app
ExecStart=/path/to/your/app/start.sh
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=service-name
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

3. Enable and start the service:

```bash
sudo systemctl enable service-name.service
sudo systemctl start service-name.service
```

4. Check status:

```bash
sudo systemctl status service-name.service
```

5. For npm scripts, it makes sense to create a start.sh file like

```bash
#!/bin/bash
. /home/alex/.nvm/nvm.sh
npm run start
```
