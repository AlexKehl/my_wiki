#Nginx Proxy Manager

## Default credentials

Email:    admin@example.com
Password: changeme

## Installation

Add to docker-compose
```yaml
  nginx-proxy-manager:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: always
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./nginx/data:/data
      - ./nginx/letsencrypt:/etc/letsencrypt
```

## Configuration
Dashboard -> Proxy Hosts -> Add Proxy Host
Add like 
Domain Names: 94.123.123.123
Scheme: http/https
Forward Hostname / IP: 94.123.123.123
Forward Port: 3000

Save and it should work
