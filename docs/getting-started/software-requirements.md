## Operating systems ##

For the best experience when running Overleaf, we highly recommend using a :simple-debian: **Debian-based** operating system, such as :simple-ubuntu: **Ubuntu**. This choice aligns with the software's development environment and is the preferred option among the majority of Overleaf users.

!!! danger "Important"

    When utilizing {{versions['server-pro-short']}} with Sandbox Compiles, it's **important** to note that the application requires root access to the Docker socket. 

## Dependencies ##

Both {{versions['community-edition-short']}} and {{versions['server-pro-short']}} currently support the following versions of dependencies:

- :simple-docker: **Docker 23.0, 24.0, 25.0 and 26.0**
- :simple-mongodb: **MongoDB 5.0**
- :simple-redis: **Redis 6**

!!! tip

    `docker-compose` is generally installed with Docker via the `docker-compose-plugin` package.

MongoDB and Redis are automatically pulled by `docker compose` when running {{versions['community-edition-short']}} or {{versions['server-pro-short']}}, unless configured to use a different installation.

The {{ versions['toolkit-full'] }} depends on the following programs:

- bash
- Docker

`docker compose` is required and is generally installed with Docker.

We recommend that you install the most recent version of Docker that is available for your operating system.

Once Docker is installed correctly, you should be able to run these commands without error:

```
# Shows the installed Docker version
docker --version

# docker compose plugin (v2)
docker compose version
# legacy docker-compose (v1)
docker-compose --version

# List the running Docker containers on your system
docker ps

CONTAINER ID   IMAGE                                     COMMAND                  CREATED        STATUS                        PORTS                NAMES
b1fadcd1dcb1   quay.io/sharelatex/sharelatex-pro:5.1.0   "/sbin/my_init"          23 hours ago   Up About a minute             0.0.0.0:80->80/tcp   sharelatex
7900ebb9ebb8   redis:6.2                                 "docker-entrypoint.s…"   45 hours ago   Up About a minute             6379/tcp             redis
fbd49d420e59   mongo:5.0                                 "docker-entrypoint.s…"   45 hours ago   Up About a minute (healthy)   27017/tcp            mongo
```

!!! tip

    The [{{versions['toolkit-full']}}](https://github.com/overleaf/toolkit) includes a handy [`bin/doctor`](https://github.com/overleaf/toolkit/blob/master/doc/the-doctor.md) script that produces a report pointing to any unfulfilled dependency.