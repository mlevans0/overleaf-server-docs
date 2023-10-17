## Operating systems ##

For the best experience when running Overleaf, we highly recommend using a **Debian-based** operating system, such as **Ubuntu**. This choice aligns with the software's development environment and is the preferred option among the majority of Overleaf users.

!!! danger "Important"

    When utilizing {{versions['server-pro-short']}} with Sandbox Compiles, it's **important** to note that the application requires root access to the Docker socket. 

## The {{ versions['toolkit-full'] }} ##

The {{ versions['toolkit-short'] }} depends on the following programs:

- bash
- Docker

`docker compose` is required and is generally installed with Docker.

We recommend that you install the most recent version of Docker that is available for your operating system.

Once Docker is installed correctly, you should be able to run these commands without error:

```
# Shows the installed Docker version
$ docker --version

Docker version 24.0.5, build ced0996

# Shows the installed Docker compose version
$ docker compose version

Docker Compose version v2.20.2

# List the running Docker containers on your system
docker ps

CONTAINER ID   IMAGE                                     COMMAND                  CREATED        STATUS                        PORTS                NAMES
b1fadcd1dcb1   quay.io/sharelatex/sharelatex-pro:4.1.0   "/sbin/my_init"          23 hours ago   Up About a minute             0.0.0.0:80->80/tcp   sharelatex
7900ebb9ebb8   redis:6.2                                 "docker-entrypoint.s…"   45 hours ago   Up About a minute             6379/tcp             redis
fbd49d420e59   mongo:4.4                                 "docker-entrypoint.s…"   45 hours ago   Up About a minute (healthy)   27017/tcp            mongo
```