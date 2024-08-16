# DevOps-Dockerfile

## CMD vs ENTRYPOINT

### CMD 

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

### ENTRYPOINT

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

