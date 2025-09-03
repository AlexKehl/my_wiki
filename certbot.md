# Certbot

1. Install Nginx as a reverse proxy:
```bash
sudo apt install nginx
```

2. Install Certbot and the Nginx plugin:
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

3. Configure Nginx as a reverse proxy to your app:
```bash
sudo nano /etc/nginx/sites-available/yourdomain.com
```

Add this configuration:
```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

4. Enable the site:
```bash
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

5. Obtain and install a certificate:
```bash
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

6. Follow the interactive prompts. Certbot will:
   - Verify domain ownership
   - Ask for your email (for renewal notifications)
   - Ask you to agree to terms of service
   - Ask if you want to redirect HTTP to HTTPS (recommended)

7. Verify auto-renewal is configured:
```bash
sudo systemctl status certbot.timer
```
