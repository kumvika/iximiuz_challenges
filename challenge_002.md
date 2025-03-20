## Problem
Run a container using the `docker run` command. That's it - that's the challenge.

## Solution

```
laborant@docker-01:~$ docker run nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
d9b636547744: Pull complete
0994e771ba34: Pull complete
bef2ee7fab45: Pull complete
13f89c653285: Pull complete
589701e352f8: Pull complete
8e77214beb25: Pull complete
4c7c1a5bd3af: Pull complete
Digest: sha256:124b44bfc9ccd1f3cedf4b592d4d1e8bddb78b51ec2ed5056c52d3692baebc19
Status: Downloaded newer image for nginx:latest
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/03/20 09:35:20 [notice] 1#1: using the "epoll" event method
2025/03/20 09:35:20 [notice] 1#1: nginx/1.27.4
2025/03/20 09:35:20 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2025/03/20 09:35:20 [notice] 1#1: OS: Linux 6.10.14-linuxkit
2025/03/20 09:35:20 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/03/20 09:35:20 [notice] 1#1: start worker processes
2025/03/20 09:35:20 [notice] 1#1: start worker process 29
2025/03/20 09:35:20 [notice] 1#1: start worker process 30
2025/03/20 09:35:20 [notice] 1#1: start worker process 31
2025/03/20 09:35:20 [notice] 1#1: start worker process 32
2025/03/20 09:35:20 [notice] 1#1: start worker process 33
2025/03/20 09:35:20 [notice] 1#1: start worker process 34
2025/03/20 09:35:20 [notice] 1#1: start worker process 35
2025/03/20 09:35:20 [notice] 1#1: start worker process 36
```

## Explanation

When we try inspecting the process tree on the host (using a separate terminal tab)
```
USER      PID    COMMAND
root        1    /sbin/init
root     1063    |\_ login
devel    1279    |    \_ -bash
devel    1697    |        \_ docker run nginx
root     1073    |\_ /usr/bin/containerd
root     1373    |\_ /usr/bin/dockerd
root     1806    |\_ /usr/bin/containerd-shim-runc-v2
root     1828    |    \_ nginx: master process
nginx    1874    |        \_ nginx: worker process
nginx    1875   ...       \_ nginx: worker process
```

Whenever we run a container using docker run, the process that keeps the terminal busy isn't the actual container process. Instead, it's the Docker CLI. The terminal communicates with the container by passing input, output, and signals through several layers: Docker, Docker's background service (dockerd), the container manager (containerd), and the runtime shim, which acts as a bridge.
