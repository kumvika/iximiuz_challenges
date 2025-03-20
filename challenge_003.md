## Problem
When you run a container using the `docker run <image>` command without any additional flags, the container starts in the foreground - the command blocks the terminal session and the container's stdout and stderr streams starts being written to the console.
While handy for some use cases (e.g., running a CLI tool in a container, quickly testing a container image, etc.), often you'd want to run the container in the background - i.e., without attaching the current shell to its stdio streams. This way you can continue using the terminal session after the container starts, and the container itself will keep running even after the terminal window is closed.

In this challenge, you'll practice:

- Running a container in the background
- Accessing the container's logs
- Re-attaching to a background container

## Solution

### To run a container in the background

```
laborant@docker-01:~$ docker run -d nginx
1daaf022ad689831ec1ab2fb20cfe1b16c1a53da2474197062a15fd43a48c8a1
```

### To access the logs of the container

```
laborant@docker-01:~$ docker logs 1daaf022ad689831ec1ab2fb20cfe1b16c1a53da2474197062a15fd43a48c8a1
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/03/20 10:06:52 [notice] 1#1: using the "epoll" event method
2025/03/20 10:06:52 [notice] 1#1: nginx/1.27.4
2025/03/20 10:06:52 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2025/03/20 10:06:52 [notice] 1#1: OS: Linux 6.10.14-linuxkit
2025/03/20 10:06:52 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/03/20 10:06:52 [notice] 1#1: start worker processes
2025/03/20 10:06:52 [notice] 1#1: start worker process 29
2025/03/20 10:06:52 [notice] 1#1: start worker process 30
2025/03/20 10:06:52 [notice] 1#1: start worker process 31
2025/03/20 10:06:52 [notice] 1#1: start worker process 32
2025/03/20 10:06:52 [notice] 1#1: start worker process 33
2025/03/20 10:06:52 [notice] 1#1: start worker process 34
2025/03/20 10:06:52 [notice] 1#1: start worker process 35
2025/03/20 10:06:52 [notice] 1#1: start worker process 36
```

### To re-attach the terminal to the container running in the background

```
laborant@docker-01:~$ docker attach 1daaf022ad689831ec1ab2fb20cfe1b16c1a53da2474197062a15fd43a48c8a1
```