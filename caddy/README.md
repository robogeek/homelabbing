
This directory explains how to install/configure Caddy as a reverse proxy in Docker, in a form suitable for a "home lab".

The model is to have either a single computer with a single Docker Engine instance, or multiple computers in a Docker Swarm configuration.

The Caddy instance will act as a reverse proxy for any Container, or for other locally installed non-Docker services.  Each container will be identified with a unique domain name.

# Simple NGINX container for debugging

I found it useful to have a simple-to-launch container with known simple characteristics for use in debugging Caddy features and configuration.

```yaml
services:
  nginx:
    image: nginx
#    ports:
#     - "8080:80"
    environment:
     - NGINX_PORT=80
    networks:
     - servernet

networks:
  servernet:
    external: true
```

This launches NGINX on port 80 with the defaults.  In that mode, NGINX shows a simple HTML page.  The point is to create a Caddy configuration section connecting to this, and the simplicity of this service allows one to easily experiment with configuration settings.

Connecting Caddy to this is very simple:

```
localhost {
    reverse_proxy ng-nginx-1
}
```

