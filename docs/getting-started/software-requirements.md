# Software requirements

The Overleaf Toolkit is the recommended way to deploy and manage {{ versions['community-edition-full'] }} and Overleaf Server Pro.

## Operating systems

For the best experience when running Overleaf, we highly recommend using a **Debian-based** operating system, such as **Ubuntu**. This choice aligns with the software's development environment and is the preferred option among the majority of Overleaf users.

We recommend a debian based operating system such as Ubuntu for running Overleaf, this is what the software has been developed using and most people use when running Overleaf.

!!! danger "Important"

    When utilizing {{versions['server-pro-short']}} with Sandbox Compiles, it's **important** to note that the application requires root access to the Docker Socket. 

## Overleaf Toolkit

The Overleaf Toolkit depends on the following programs:

- bash
- Docker

`docker compose` is required and is generally installed with Docker.

We recommend that you install the most recent version of docker that is available for your operating system.

Once docker is installed correctly, you should be able to run these commands without error:

```
# Shows the installed docker version
$ docker --version

Docker version 24.0.5, build ced0996

# Shows the installed docker compose version
$ docker compose version

Docker Compose version v2.20.2

# List the running Docker containers on your system
docker ps

CONTAINER ID   IMAGE                                     COMMAND                  CREATED        STATUS                        PORTS                NAMES
b1fadcd1dcb1   quay.io/sharelatex/sharelatex-pro:4.1.0   "/sbin/my_init"          23 hours ago   Up About a minute             0.0.0.0:80->80/tcp   sharelatex
7900ebb9ebb8   redis:6.2                                 "docker-entrypoint.s…"   45 hours ago   Up About a minute             6379/tcp             redis
fbd49d420e59   mongo:4.4                                 "docker-entrypoint.s…"   45 hours ago   Up About a minute (healthy)   27017/tcp            mongo
```

The Overleaf Toolkit includes a handy `bin/doctor` script that produces a report pointing to any unfulfilled dependency.

```
====== Overleaf Doctor ======
- Host Information
    - Linux
    - Output of 'lsb_release -a':
            No LSB modules are available.
            Distributor ID:     Ubuntu
            Description:        Ubuntu 22.04.3 LTS
            Release:    22.04
            Codename:   jammy
- Dependencies
    - bash
        - status: present
        - version info: 5.1.16(1)-release
    - docker
        - status: present
        - version info: Docker version 24.0.5, build ced0996
    - realpath
        - status: present
        - version info: realpath (GNU coreutils) 8.32
    - perl
        - status: present
        - version info: 5.034000
    - awk
        - status: present
        - version info: GNU Awk 5.1.0, API: 3.0 (GNU MPFR 4.1.0, GNU MP 6.2.1)
    - openssl
        - status: present
        - version info: OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
    - docker compose
        - status: present
        - version info: Docker Compose version v2.20.2
- Docker Daemon
    - status: up
====== Configuration ======
- config/version
    - status: present
    - version: 4.1.0
- config/overleaf.rc
    - status: present
    - values
        - SHARELATEX_DATA_PATH: data/sharelatex
        - SERVER_PRO: true
            - logged in to quay.io: true
        - SIBLING_CONTAINERS_ENABLED: true
        - SHARELATEX_LISTEN_IP: 0.0.0.0
        - SHARELATEX_PORT: 80
        - MONGO_ENABLED: true
        - MONGO_IMAGE: mongo:4.4
        - MONGO_DATA_PATH: data/mongo
        - REDIS_ENABLED: true
        - REDIS_IMAGE: redis:6.2
        - REDIS_DATA_PATH: data/redis
        - NGINX_ENABLED: false
        - NGINX_CONFIG_PATH: config/nginx/nginx.conf
        - TLS_PRIVATE_KEY_PATH: config/nginx/certs/overleaf_key.pem
        - TLS_CERTIFICATE_PATH: config/nginx/certs/overleaf_certificate.pem
        - NGINX_HTTP_LISTEN_IP: 127.0.1.1
        - NGINX_HTTP_PORT: 80
        - NGINX_TLS_LISTEN_IP: 127.0.1.1
        - TLS_PORT: 443
        - GIT_BRIDGE_ENABLED: false
- config/variables.env
    - status: present
    - values
        - SHARELATEX_FILESTORE_BACKEND: fs
        - SHARELATEX_HISTORY_BACKEND: fs
====== Warnings ======
- None, all good
====== End ======

```

!!! tip "Need help?"

    If you require assistance with your **{{versions['server-pro-short']}}** instance, kindly include a copy of the output from running `bin/doctor` in your email so that we are able to help you with your query.