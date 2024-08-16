# DevOps-Dockerfile

## 1. FROM

Specifies the base image from which you are building.
Example: FROM ubuntu:20.04

## 2. ARG

Defines a build-time variable. You can pass values to these variables when you build the Docker image using --build-arg.

## 3. ENV

Sets environment variables that are available in the container during runtime.

ENV NODE_ENV=production

## 4. LABEL

Adds metadata to an image, such as a version number, description, or maintainer information.

LABEL version="1.0" maintainer="you@example.com"

**code**

```
# Base image
FROM nginx:alpine

# Set build-time variable
ARG NGINX_VERSION=1.21.0

# Set environment variables
ENV NGINX_HOME=/etc/nginx

# Add metadata to the image
LABEL maintainer="Latchu <slakshminarayanan@live.com>"
LABEL version=$NGINX_VERSION
LABEL description="A Docker image to run Nginx version $NGINX_VERSION on port 80."

# Expose the default port for Nginx
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]

docker build -t mynginx .
docker run -d -it --name mynginx -p 80:80 mynginx
```

## 5. RUN

Executes a command in the shell during the build process. It’s used to install packages, set up software, etc.

RUN apt-get update && apt-get install -y curl

## 6. COPY

Copies files or directories from the host system into the image’s filesystem.

COPY ./myapp /app

**code**

```
# Base image
FROM nginx:alpine

# Set build-time variable
ARG NGINX_VERSION=1.21.0

# Set environment variables
ENV NGINX_HOME=/etc/nginx

# Add metadata to the image
LABEL maintainer="Latchu <slakshminarayanan@live.com>"
LABEL version=$NGINX_VERSION
LABEL description="A Docker image to run Nginx version $NGINX_VERSION on port 80 with a custom index.html."

# Set the working directory
WORKDIR /usr/share/nginx/html

# Copy the custom index.html to the Nginx default directory
COPY index.html /usr/share/nginx/html/index.html

# Expose the default port for Nginx
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]

docker build -t mynginx .
docker run -d -it --name mynginx -p 80:80 mynginx
```

**index.html**

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome</title>
</head>
<body>
    <h1>Hi Latchu</h1>
</body>
</html>
```

## 7. ADD

Similar to COPY, but with additional features like extracting tar files and supporting URLs.

ADD myarchive.tar /app

## 8. WORKDIR

Sets the working directory inside the container for subsequent RUN, CMD, ENTRYPOINT, COPY, and ADD instructions.

WORKDIR /app

**code**

```
# Base image
FROM nginx:alpine

# Set build-time variable
ARG NGINX_VERSION=1.21.0

# Set environment variables
ENV NGINX_HOME=/etc/nginx

# Add metadata to the image
LABEL maintainer="Latchu <slakshminarayanan@live.com>"
LABEL version=$NGINX_VERSION
LABEL description="A Docker image to run Nginx version $NGINX_VERSION on port 80 with a custom index.html from a zip file."

# Set the working directory
WORKDIR /usr/share/nginx/html

# Add and extract the zip file
ADD index.html.tar /usr/share/nginx/html/

# Expose the default port for Nginx
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
```

## 9. EXPOSE

Informs Docker that the container will listen on the specified network ports at runtime. This doesn’t actually publish the ports; that’s done with docker run -p.

EXPOSE 8080


## CMD vs ENTRYPOINT

## 10. CMD 

**code**

```
FROM debian:wheezy
CMD ["/bin/ping", "localhost"]

docker build -t test .
```

**without argument**

```
docker run -it test

PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_req=1 ttl=64 time=0.029 ms
64 bytes from localhost (127.0.0.1): icmp_req=2 ttl=64 time=0.038 ms
64 bytes from localhost (127.0.0.1): icmp_req=3 ttl=64 time=0.041 ms
```

**with argument**

```
docker run -it test /bin/bash

ubuntu@ip-172-31-28-57:~/dokcerfile$ docker run -it test /bin/bash
root@d78e72ec6c73:/#
```

## 11. ENTRYPOINT

**code**

```
FROM debian:wheezy
ENTRYPOINT ["/bin/ping", "localhost"]

docker build -t test .
```

**without argument**

```
docker run -it test

PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_req=1 ttl=64 time=0.027 ms
64 bytes from localhost (127.0.0.1): icmp_req=2 ttl=64 time=0.039 ms
```

**with argument**

```
docker run --entrypoint /bin/bash -it test

root@06151f2cfa7d:/#
```

## 12. ENTRYPOINT with CMD

**code**

```
FROM debian:wheezy
ENTRYPOINT ["/bin/ping"]
CMD ["localhost"]

docker build -t test .
```

**without argument**

```
docker run -it test

PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_req=1 ttl=64 time=0.026 ms
64 bytes from localhost (127.0.0.1): icmp_req=2 ttl=64 time=0.038 ms
64 bytes from localhost (127.0.0.1): icmp_req=3 ttl=64 time=0.037 ms
64 bytes from localhost (127.0.0.1): icmp_req=4 ttl=64 time=0.037 ms
```

**with argument**

```
docker run -it test google.com

PING google.com (172.217.27.206) 56(84) bytes of data.
64 bytes from bom07s15-in-f14.1e100.net (172.217.27.206): icmp_req=1 ttl=53 time=1.51 ms
64 bytes from bom07s15-in-f14.1e100.net (172.217.27.206): icmp_req=2 ttl=53 time=1.56 ms
64 bytes from bom07s15-in-f14.1e100.net (172.217.27.206): icmp_req=3 ttl=53 time=1.57 ms
```

