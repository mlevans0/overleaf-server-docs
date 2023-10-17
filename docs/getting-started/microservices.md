The recommended way to deploy and manage Overleaf {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }} instances is via the use of the {{ versions['toolkit-full'] }}. 

The {{ versions['toolkit-full'] }} simplifies the creation of your Overleaf instance through the use of some custom scripts that abstract away the orchestration of required microservices. Simply run the bundled initialization script, provide a few configuration options like your persistent storage paths, and the {{ versions['toolkit-full'] }} will take care of provisioning and connecting the microservices that make up your Overleaf {{ versions['community-edition-short'] }} or {{ versions['server-pro-short'] }} instance. 

This leaves you free to focus on customizing the user experience and implementing the specific features that make up your on-premise instance. The {{ versions['toolkit-full'] }} handles all the complexity behind the scenes, enabling a simplified deployment of your Overleaf instance.

## Working with Docker Compose services

The {{ versions['toolkit-full'] }} runs your instance inside a Docker container, plus the supporting databases (MongoDB and Redis), each in their own containers. All of this is orchestrated with `docker compose`.

!!! note

    For legacy reasons, the main Overleaf container is called `sharelatex`, and is based on the `sharelatex/sharelatex` Docker image. This is because the technology is based on the ShareLaTeX code base, which was merged into Overleaf. See [this blog post](https://www.overleaf.com/blog/518-exciting-news-sharelatex-is-joining-overleaf) for more details. At some point in the future, this will be renamed to match the Overleaf naming scheme.

## Architecture

Inside the Overleaf container, the software runs as a set of microservices, managed by `runit`. Some of the more interesting files inside the container are:

- `/etc/service/`: initialization files for the microservices.
- `/etc/sharelatex/settings.coffee`: unified settings file for the microservices.
- `/var/log/sharelatex/`: logs for each microservice.
- `/var/www/sharelatex/`: code for the various microservices.
- `/var/lib/sharelatex/`: the mount-point for persistent data (corresponds to the directory indicated by `SHARELATEX_DATA_PATH` on the host).

## The MongoDB and Redis Containers

Overleaf depends on two external databases: MongoDB and Redis. By default, the {{ versions['toolkit-short'] }} will provision a container for each of these databases, in addition to the Overleaf container, for a total of three Docker containers.

!!! tip

    If you would prefer to connect to an existing MongoDB or Redis instance, you can do so by setting the appropriate settings in the [overleaf.rc](./overleaf-rc.md) configuration file.
