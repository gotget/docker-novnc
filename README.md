# noVNC-Docker image
>  [noVNC](http://novnc.com/) is a [VNC](https://en.wikipedia.org/wiki/Virtual_Network_Computing) client using HTML5 (Web Sockets, Canvas) with encryption (wss://) support.
>  
>  *[noVNC-Docker](https://hub.docker.com/r/gotget/novnc/)* is a Docker image that bundles the [noVNC client](https://github.com/novnc/noVNC) and WebSocket-to-TCP proxy/bridge, [websockify](https://github.com/novnc/websockify), for ease-of-use.

## Download Docker image

### Pull from Docker Hub
```
docker pull gotget/novnc
```

### Build from GitHub
Since there's no external files that are added in, it's easy to build a local image directly from GitHub:
```
docker build --tag gotget/novnc https://raw.githubusercontent.com/gotget/docker-novnc/master/Dockerfile
```

## Examples

### Docker run

#### HTTP
```
docker run \
    --name noVNC \
    --detach \
    --publish 80:6080 \
    gotget/novnc \
        --vnc HOST:PORT
```

#### HTTPS
```
docker run \
    --name noVNC \
    --detach \
    --publish 443:6080 \
    gotget/novnc \
        --cert server.pem \
        --ssl-only \
        --vnc HOST:PORT
```

### Docker Compose
Maybe you'd like to have a [reverse proxy from Nginx to noVNC](https://github.com/novnc/noVNC/wiki/Proxying-with-nginx)?  This is easy to accomplish with [Docker Compose](https://docs.docker.com/compose/), and you'll notice the format resembles that of the article, [Thad.Getterman.org : Adding Django to Nginx with Docker](https://thad.getterman.org/2017/09/14/adding-django-to-nginx-with-docker).
```
---
version: '3'

services:

vnc_client1:

    # VNC Client 1 : Variables
    container_name: vnc_client1
    hostname: vnc_client1
    command: --vnc HOST:PORT
    networks:
        - frontend

    # VNC Client 1 : Constants
    image: gotget/novnc
    restart: always
    expose:
        - 6080/tcp

web:

    # Variables
    container_name: nginx
    volumes:
        - /srv/letsencrypt/etc/:/etc/letsencrypt/:ro
        - /srv/nginx/etc/:/etc/nginx/:ro
        - /srv/nginx/log/:/var/log/nginx/
        - /srv/sites/:/sites/:ro
        - /srv/sites/default/:/usr/share/nginx/html/:ro
    networks:
        - frontend

    # Constants
    image: nginx:latest
    restart: always
    ports:
        - 80:80      # HTTP
        - 443:443    # HTTPS

networks:

    frontend:
        driver: bridge

```

## Authors/Contributors
* [Louis T. Getterman IV](https://Thad.Getterman.org/about)
* Have an improvement? Your name goes here!

> Written with [StackEdit](https://stackedit.io/).
