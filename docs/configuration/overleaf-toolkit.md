# Files and locations

This page describes the configuration files that are used to configure the {{ versions['toolkit-full'] }}.

## Configuration File Location ##

All user-owned configuration files are found in the `config/` directory.

This directory is **excluded** from the git revision control system, so it will not be changed by updating the {{ versions['toolkit-short'] }} code. The {{ versions['toolkit-short'] }} will **not** change any data in the `config/` directory without your permission.

!!! note

    Changes to the configuration files will not be automatically applied to existing containers, even if the container is stopped and restarted (with
    `bin/stop` and `bin/start`). To apply the changes, run `bin/up`, and `docker compose` will automatically apply the configuration changes to a new container. (Or, run `bin/up -d`, if you prefer to not attach to the Docker logs)

### The `overleaf.rc` File ##

The `config/overleaf.rc` file contains the most important **top level** configuration settings used by the {{ versions['toolkit-short'] }}. It contains statements that set variables, in the format `VARIABLE_NAME=value`.

To see a breakdown of all available configuration options see our [{{ versions['toolkit-short'] }} settings](toolkit-settings.md) section.

### The `variables.env` File ##

The `config/variables.env` file contains environment variables that are loaded into the `sharelatex` container, and used to configure the Overleaf microservices. These include the name of the application, as displayed in the header of the web interface, settings for sending emails, and other premium settings such as SSO for use with {{ versions['server-pro-short'] }}.

To see a breakdown of all available environment variables see our [Environment variables](environment-variables.md) section.

### The `version` File ##

The `config/version` file contains the version number of the Docker image that will be used to create the running instance of your Overleaf server.

!!! note

    Changes to these configuration files will not be automatically applied to existing containers, even if the container is stopped and restarted (with `bin/stop` and `bin/start`). 
    
    To apply your changes, run `bin/up`, and the {{ versions['toolkit-short'] }} will automatically create a new container for you with configuration changes applied. 

### The `docker-compose.override.yml` File ###

If present, the `config/docker-compose.override.yml` file will be included in the invocation to `docker compose`. This is useful for overriding configuration specific to Docker compose.

See the [docker-compose documentation](https://docs.docker.com/compose/extends/#adding-and-overriding-configuration) for more details.