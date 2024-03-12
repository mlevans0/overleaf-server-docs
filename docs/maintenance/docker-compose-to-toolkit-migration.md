# docker-compose.yml to Toolkit migration #

If you're currently using Docker Compose via a `docker-compose.yml` file, migrating to the Toolkit can make running an on-premises version of Overleaf easier to deploy, upgrade and maintain.

To upgrade, you'll need to convert your existing Docker Compose setup into the format used by the Toolkit. This process involves copying existing configuration into the Toolkit.

This guide will walk you through each step of this process, ensuring a smooth migration from Docker Compose to the Toolkit.

!!! note

    These instructions are for v4.x and earlier. Therefore all variables use the `SHARELATEX_` prefix instead of `OVERLEAF_`

## Clone the Toolkit repository ##

First, let's clone this Toolkit repository to the host machine:
```
$ git clone https://github.com/overleaf/toolkit.git ./overleaf-toolkit
```
Next run the `bin/init` command to initialise the Toolkit with its default configuration.

## Setting the image and version ##

In the `docker-compose.yml` file the image and version are defined in the component description:

```
version: '2.2'
services:
    sharelatex:
        restart: always
        # Server Pro users:
        # image: quay.io/sharelatex/sharelatex-pro
        image: sharelatex/sharelatex:3.5.13
```

When using the Toolkit, the image name is automatically resolved; the only requirement is to set `SERVER_PRO=true` in **config/overleaf.rc** to pick the Server Pro image or `SERVER_PRO=false` to use Community Edition.

The desired version number is set in the **config/version** file. We recommend avoiding the use of "**latest**", and instead using a specific version number like "**4.2.3**".

If you are sourcing the image from your own internal registry you can override the image the Toolkit uses by setting `SHARELATEX_IMAGE_NAME`. You do not need to specify the tag as the Toolkit will automatically add it based on your **config/version** file.

## Configuring external access ##

By default, Overleaf will listen on **127.0.0.1:80**, only allowing traffic from the Docker host machine.

To allow external access, you’ll need to set the `SHARELATEX_LISTEN_IP` and `SHARELATEX_PORT` in the [config/overleaf.rc](/configuration/overleaf-toolkit/#the-overleafrc-file) file.

## Environment variable migration ##

You’ll likely have a set of environment variables defined in the **sharelatex** service:

```
environment:
    SHARELATEX_APP_NAME: Overleaf Community Edition
    SHARELATEX_PROXY_LEARN: 'true'
    …
```

Each of these variables should be copied, with several exceptions we’ll list later, into the Toolkit’s **config/variables.env** file, ensuring the following form (note the use of `=` instead of `:`):

```
SHARELATEX_APP_NAME=Overleaf Community Edition
SHARELATEX_PROXY_LEARN=true
```

As mentioned above, there are several exceptions, as certain features are configured differently when using the Toolkit:

- Variables starting with `SANDBOXED_COMPILES_` and `DOCKER_RUNNER` are no longer needed. To enable [Sandboxed Compiles](./sandboxed-compiles.md), set `SIBLING_CONTAINERS_ENABLED=true` in your **config/overleaf.rc** file.
- Variables starting with `SHARELATEX_MONGO_`, `SHARELATEX_REDIS_` and the `REDIS_HOST` variable are no longer needed. MongoDB and Redis are now configured in the **config/overleaf.rc** file using  `MONGO_URL`, `REDIS_HOST` and `REDIS_PORT`.

For advanced configuration options, refer to the [config/overleaf.rc](/configuration/overleaf-toolkit/#the-overleafrc-file) documentation.

## Volumes ##

### MongoDB ###

The MongoDB volume will need to be set using `MONGO_DATA_PATH` in the **config/overleaf.rc** file.

### Redis ###

The Redis volume will need to be set using `REDIS_DATA_PATH` in the **config/overleaf.rc** file.
