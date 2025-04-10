
This directory explains how to install/configure Caddy as a reverse proxy in Docker, in a form suitable for a "home lab".

The model is to have either a single computer with a single Docker Engine instance, or multiple computers in a Docker Swarm configuration.

The Caddy instance will act as a reverse proxy for any Container, or for other locally installed non-Docker services.  Each container will be identified with a unique domain name.

# Simple Caddy container

The Caddy container described in [`compose.yml`](./compose.yml) is derived from documentation for the [official Caddy image](https://hub.docker.com/_/caddy) on Docker Hub.

A few small changes were made due to differences of opinion.

**Defined volumes**: I dislike the use of anonymous volumes, and prefer to explicitly define the file system location for volumes.  Volumes are required to ensure persistence of data.  But, with a volume mount defined as `caddy_data:/data` it's unclear what is the host filesystem location for the volume data.  Yes, the data is persisted outside the container, but for that data to be useful it must be in a known location.  Yes, the `docker volume inspect` command can be used to find the file-system location.  But, if you explicitly declare that location then there's no doubt about where it is stored.

The volume mounts are specified as:

```yaml
  volumes:
    - $DOCKER_BASE/docker/caddy/conf:/etc/caddy
    - $DOCKER_BASE/docker/caddy/site:/srv
    - $DOCKER_BASE/docker/caddy/data:/data
    - $DOCKER_BASE/docker/caddy/config:/config
```

The value for `DOCKER_BASE` might be `/opt` or `/home/USER`.  Within that directory, `docker` is used for all containers, with each container being stored in its own directory.

**Network configuration**: Caddy is to be used as a reverse-proxy for Containers which are connected to Caddy.  The way we'll do that is with the `servernet` bridge network.  Any container to expose to the Internet is attached to that network.  The Caddy configuration can then refer to the container by its name.


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

