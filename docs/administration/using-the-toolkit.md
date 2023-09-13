The {{ versions['toolkit-short'] }} uses `docker compose` to manage your servers Docker containers. The {{ versions['toolkit-short'] }} provides you with a set of scripts which wrap `docker compose`, and helps take care of most of the more technical details for you.
    
## The `bin/docker-compose` Wrapper

The `bin/docker-compose` script is a wrapper around `docker compose`. It
loads configuration from the `config/` directory, before invoking
`docker compose` with whatever arguments were passed to the script.

You can treat `bin/docker-compose` as a transparent wrapper for the
`docker compose` program installed on your machine.

For example, we can check which containers are running with the following:

```
$ bin/docker-compose ps
```

## Convenience Helpers

In addition to `bin/docker-compose`, the toolkit also provides a collection of
convenient scripts to automate common tasks:

- `bin/up`: shortcut for `bin/docker-compose up`
- `bin/start`: shortcut for `bin/docker-compose start`
- `bin/stop`: shortcut for `bin/docker-compose stop`
- `bin/shell`: starts a shell inside the main container

backup-config  dev  docker-compose  doctor  error-logs  images  init  logs  mongo  shell  start  stop  up  upgrade

!!! tip    

    If you prefer to run your instance without attaching to the Docker logs you can run `bin/up -d` to run in detached mode.

## Checking your server

The {{ versions['toolkit-short'] }} includes a handy script called `bin/doctor` that produces a report pointing to any unfulfilled dependency.

Before we continue any further, let's run the `bin/doctor` script and check that everything is working correctly.

```
bin/doctor
```

We should see some output similar to this:

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
- config/variables.env
    - status: present
    - values
        - SHARELATEX_FILESTORE_BACKEND: fs
        - SHARELATEX_HISTORY_BACKEND: fs
====== Warnings ======
- None, all good
====== End ======

```

First, we see some information about the host system (the machine that the {{ versions['toolkit-short'] }} is being run on), then some information about dependencies. If any dependencies are missing, we will see a warning here. Next, the doctor checks our local configuration. At the end, the doctor will print out some warnings, if any problems were encountered.

If you run into problems with running the {{ versions['toolkit-short'] }}, you should first run the `bin/doctor` script and check it's output for any warnings.

!!! tip "Need help?"

    Users of the free {{ versions['community-edition-short'] }} should open an issue on [GitHub](https://github.com/overleaf/toolkit/issues).

    Users of {{ versions['server-pro-short'] }} should contact [support@overleaf.com](mailto:support@overleaf.com) for assistance.

    In both cases, it is a good idea to include the output of the `bin/doctor` script in your message.    
