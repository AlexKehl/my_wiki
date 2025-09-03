# Squid

Allows making vps a proxy server

## Install with docker

```bash
mkdir -p ~/squid

docker run -d --name squid -p 3128:3128 ubuntu/squid:5.2-22.04_beta \ 
    -v ~/squid/conf:/etc/squid/squid.conf
    
docker run -d --name squid \
  -p 3128:3128 \
  -v ~/squid/conf/squid.conf:/etc/squid/squid.conf \
  sameersbn/squid
```
