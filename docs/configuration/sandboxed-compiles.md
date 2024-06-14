---
tags:
  - Server Pro
---

{{ versions['server-pro-short'] }} comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured Docker environment. With Sandboxed Compiles you also benefit from easier package management. 

## Improved security ##

Sandbox Compiles are the recommended approach for {{ versions['server-pro-short'] }} due to many LaTeX documents requiring/having the ability to execute arbitrary shell commands as part of the PDF compile process. If you use Sandboxed Compiles, each compile runs in a separate Docker container with limited capabilities that are not shared with any other user or project and has no access to outside resources such as the host network. 

If you attempt to run {{ versions['server-pro-short'] }} without Sandboxed Compiles, the compile runs alongside other concurrent compiles inside the main Docker container.

## Easier package management ##

To avoid manually installing packages, we recommend enabling Sandbox Compiles. This is a configurable setting within {{ versions['server-pro-short'] }} that will provide your users with access to the same Tex Live environment as that on [overleaf.com](overleaf.com) but within your own on-premise installation. TeX Live images used by Sandboxed Compiles contain the most popular packages and fonts tested against our gallery templates, ensuring maximum compatibility with on-premise projects. 

Enabling Sandboxed Compiles allows you to configure which TeX Live versions users are able to choose from within their project along with setting a default TeX Live image version for new projects. 

!!! note

    If you attempt to run {{ versions['server-pro-short'] }} without Sandboxed Compiles, your instance will default to using a basic scheme version of TeX Live for compiles. This basic version is lightweight and only contains a very limited subset of LaTex packages, which will most likely result in missing package errors for your users, especially if they try to use pre-built templates. 

As {{ versions['server-pro-short'] }} has been architected to work offline, there isn't an automated way to integrate [overleaf.com](overleaf.com) gallery templates into your on-premise installation, it is, however, possible to do this manually on a per-template basis. For more information on how this works, pkeae check out our guide here: TODO:: ADD GUIDE.

!!! information

    Sandbox Compiles requires that the {{ versions['server-pro-short'] }} container has access to the Docker socket on the host machine (via a bind mount) so it can manage these compile containers.

## How it works ##

When Sandboxed Compiles are enabled, the Docker socket will be mounted from the host machine into the `sharelatex` container, so that the compiler service in the container can create new Docker containers on the host. Then for each run of the compiler in each project, the LaTeX compiler service (CLSI) will do the following:

- Write out the project files to a location inside the `OVERLEAF_DATA_PATH`, 
- Use the mounted Docker socket to create a new `texlive` container for the compile run
- Have the `texlive` container read the project data from the location under `OVERLEAF_DATA_PATH`
- Compile the project inside the `texlive` container

## Enabling Sandboxed Compiles ##

### {{ versions['toolkit-short'] }} ###

In `config/overleaf.rc`, set `SIBLING_CONTAINERS_ENABLED=true`, and ensure that the `DOCKER_SOCKET_PATH` setting is set to the location of the Docker socket on the host machine.

The next time you start the Docker services (with `bin/up`), the `sharelatex` container will verify that it can communicate with Docker on the host machine, and will pull all configured `texlive` images it requires to create the Sandboxed Compile containers. This process can take several minutes, and compiles will be un-available during this time.

TODO: Add steps to pull images first to minimize downtime

### Docker Compose ###

In this example, note the following:

- the Docker socket volume mounted into the `sharelatex` container
- `DOCKER_RUNNER` set to "true"
- `SANDBOXED_COMPILES` set to "true"
- `SANDBOXED_COMPILES_SIBLING_CONTAINERS` set to "true"
- `SANDBOXED_COMPILES_HOST_DIR` set to "/data/overleaf_data/data/compiles", the place on the host where the compile data will be written

!!! important

    Starting with Overleaf {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} `5.0.3` environment variables have been rebranded from `SHARELATEX_*` to `OVERLEAF_*`.

If you're using a `4.x` version (or earlier) please make sure the variables are prefix accordingly (e.g. `SHARELATEX_MONGO_URL` instead of `OVERLEAF_MONGO_URL`)

```yaml
version: '2'
services:
    sharelatex:
        restart: always
        image: quay.io/sharelatex/sharelatex-pro:5.0.3
        container_name: sharelatex
        depends_on:
            - mongo
            - redis
        ports:
            - 80:80
        links:
            - mongo
            - redis
        volumes:
            - /data/overleaf_data:/var/lib/overleaf # (/var/lib/sharelatex for versions 4.x and earlier)
            - /var/run/docker.sock:/var/run/docker.sock    #### IMPORTANT
        environment:
            OVERLEAF_MONGO_URL: mongodb://mongo/sharelatex
            OVERLEAF_REDIS_HOST: redis
            OVERLEAF_APP_NAME: 'My Overleaf'
            OVERLEAF_SITE_URL: "http://overleaf.mydomain.com"
            OVERLEAF_NAV_TITLE: "Our Overleaf Instance"
            OVERLEAF_ADMIN_EMAIL: "support@example.com"
            ...
            DOCKER_RUNNER: "true"
            SANDBOXED_COMPILES: "true"
            SANDBOXED_COMPILES_SIBLING_CONTAINERS: "true"    #### IMPORTANT
            # Note: (/var/lib/sharelatex for versions 4.x and earlier)
            SANDBOXED_COMPILES_HOST_DIR: "/data/overleaf_data/data/compiles"  #### IMPORTANT 
```

## Changing the Tex Live Image ##

{{ versions['server-pro-short'] }} uses three environment variables to determine which Tex Live images to use for Sandboxed Compiles: 

- `TEX_LIVE_DOCKER_IMAGE`: name of the default image for new projects
- `ALL_TEX_LIVE_DOCKER_IMAGES`: comma-separated list of all images in use
- `ALL_TEX_LIVE_DOCKER_IMAGE_NAMES`: **Optional**: comma-separated list of user-facing friendly names for the images. If omitted, the version number will be used, for example, `texlive-full:2018.1`

When the {{ versions['server-pro-short'] }} instance starts up, it will pull all of the images listed in `ALL_TEX_LIVE_DOCKER_IMAGES`. 

The current default is `quay.io/sharelatex/texlive-full:2017.1`, but you can override these values in the `config/variables.env` if you are using the Toolkit or  the `environment` section of the `docker-compose.yml' file if you are using Docker Compose.

Here's an example where we default to TeX Live 2023 for new projects, and keep both 2023 and 2022 in use for older projects:

```
    TEX_LIVE_DOCKER_IMAGE: "quay.io/sharelatex/texlive-full:2023.1"
    ALL_TEX_LIVE_DOCKER_IMAGES: "quay.io/sharelatex/texlive-full:2023.1,quay.io/sharelatex/texlive-full:2022.1"
    ALL_TEX_LIVE_DOCKER_IMAGE_NAMES: "TeX Live 2023.1,TeX Live 2022.1"
```
!!! note

    Before updating to a newer version of Tex Live we strongly recommend [performing a consistent back](/maintenance/data-and-backups#performing-a-consistent-backup) of your data and [updating to the latest version of {{ versions['server-pro-short'] }} available](https://github.com/overleaf/overleaf/wiki/Server-Pro:-setup#updating-image-versions).

### Available Tex Live images ###

These are all the Tex Live images that can be added to `TEX_LIVE_DOCKER_IMAGE` and `ALL_TEX_LIVE_DOCKER_IMAGES`:

- `quay.io/sharelatex/texlive-full:2023.1`
- `quay.io/sharelatex/texlive-full:2022.1`
- `quay.io/sharelatex/texlive-full:2021.1`
- `quay.io/sharelatex/texlive-full:2020.1` (legacy)
- `quay.io/sharelatex/texlive-full:2019.1` (legacy)
- `quay.io/sharelatex/texlive-full:2018.1` (legacy) 
- `quay.io/sharelatex/texlive-full:2017.1` (legacy)
- `quay.io/sharelatex/texlive-full:2016.1` (legacy)
- `quay.io/sharelatex/texlive-full:2015.1` (legacy)
- `quay.io/sharelatex/texlive-full:2014.2` (legacy)

## Extending Tex Live ##

It's possible to extend an existing Tex Live image using a new Dockerfile and configure the application to use the new image. 

Here we offer some guidelines to install new packages or fonts, but the configuration of a custom image is not covered by our support terms. 

The TeX Live images receive infrequent updates. We suggest rebuilding custom images when upgrading {{ versions['server-pro-short'] }}.

### Installing and updating new packages ###

You can use `tlmgr` commands such as `tlmgr install` and `tlmgr update` to manage Tex Live packages as in the following example:

```dockerfile
FROM quay.io/sharelatex/texlive-full:2023.1

RUN tlmgr update --force ebproof
```

### Using `tlmgr` in an older TeX Live image ###

By default `tlmgr` downloads resources from the latest TeX Live release. When patching an older TeX Live image, the downloads need to be switched to the respective archive. See the list in https://www.tug.org/historic/ for mirrors of archives.

```dockerfile
FROM quay.io/sharelatex/texlive-full:2022.1

RUN tlmgr option repository <MIRROR>/systems/texlive/<YEAR>/tlnet-final
# e.g. RUN tlmgr option repository ftp://tug.org/historic/systems/texlive/2022/tlnet-final

RUN tlmgr update --force ebproof
```

### Installing new fonts ###

There are different procedures to install new fonts in a Tex Live distribution, and installing a custom font might require several steps. Checking the [instructions to install TeX fonts](https://tug.org/fonts/fontinstall.html) in the official Tex Live documentation is probably a good starting point.

The following `Dockerfile` shows an example of installing a TrueType font over an existing Tex Live 2022 image:

```dockerfile
FROM quay.io/sharelatex/texlive-full:2022.1

COPY ./myfonts/*.ttf /usr/share/fonts/truetype/myfont/

# rebuild font information cache
RUN fc-cache
```

### Configuring {{ versions['server-pro-short'] }} to use the new images ###

Use the name `quay.io/sharelatex/texlive-full` and a custom tag to build the new image, as in:

```
docker build -t quay.io/sharelatex/texlive-full:2023.1-custom
```

We can now configure {{ versions['server-pro-short'] }} to use the new `2023.1-custom` image updating the `TEX_LIVE_DOCKER_IMAGE` and `ALL_TEX_LIVE_DOCKER_IMAGES` environment variables:

```
TEX_LIVE_DOCKER_IMAGE: "quay.io/sharelatex/texlive-full:2023.1-custom"
ALL_TEX_LIVE_DOCKER_IMAGES: "quay.io/sharelatex/texlive-full:2023.1,quay.io/sharelatex/texlive-full:2023.1-custom"
```

In the example above new projects are set by default to use the new `2023.1-custom` image, while `2023.1` is still available for when it's needed.