This document describes the configuration files that are used to configure the Overleaf Toolkit.

#### The overleaf.rc File

The `config/overleaf.rc` file contains the most important **top level** configuration settings used by the {{ versions['toolkit-short'] }}. It contains statements that set variables, in the format `VARIABLE_NAME=value`.

To see a breakdown of all available configuration options see our [Configuration]() section.

#### The variables.env File

The `config/variables.env` file contains environment variables that are loaded into the Overleaf Docker container, and used to configure the Overleaf microservices. These include the name of the application, as displayed in the header of the web interface, settings for sending emails, and other premium settings such as SSO for use with {{ versions['server-pro-short'] }}.

To see a breakdown of all available environment variables see our [Configuration]() section.

#### The version File

The `config/version` file contains the version number of the Docker image that will be used to create the running instance of your Overleaf server.

!!! note

    Changes to these configuration files will not be automatically applied to existing containers, even if the container is stopped and restarted (with `bin/stop` and `bin/start`). 
    
    To apply your changes, run `bin/up`, and the {{ versions['toolkit-short'] }} will automatically create a new container for you with configuration changes applied. 