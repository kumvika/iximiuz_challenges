## Problem Statement

When invoked without any arguments, the `docker run redis` command will start a `redis` server in a container. However, the redis image also includes the `redis-cli` executable, which can be used to interact with any Redis server instance (not necessarily the one running in the same container). This makes a redis container a handy way to run `redis-cli` without installing it on your machine. But only if you know how to override its default command 😉

Run `redis-cli` with the `--version` flag in a `redis` container

## Solution

```
laborant@docker-01:~$ docker run redis redis-cli --version
redis-cli 7.4.2
```

