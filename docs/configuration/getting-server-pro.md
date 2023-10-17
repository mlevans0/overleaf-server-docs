## Obtaining {{ versions['server-pro-short'] }} Image ##

{{ versions['server-pro-short'] }} is distributed as a Docker image on the [quay.io](https://quay.io) registry: `quay.io/sharelatex/sharelatex-pro`

You will have been supplied with a set of credentials when you signed up for a {{ versions['server-pro-short'] }} license.

First use your {{ versions['server-pro-short'] }} credentials to log in to quay.io:

```
docker login quay.io
Username: <sharelatex+your_key_name>
Password: <your key>
```

Then run `bin/docker-compose pull` to pull the image from the `quay.io` registry.

## Switching to {{ versions['server-pro-short'] }} ##

We recommend first setting up your {{ versions['toolkit-short'] }} with the default {{ versions['community-edition-short'] }} image before switching to {{ versions['server-pro-short'] }}.

{{ versions['toolkit-short'] }} deployments can enable {{ versions['server-pro-short'] }} by editing the `config/overleaf.rc` file and changing the `SERVER_PRO` environment variable to `true`:

```
SERVER_PRO=true
```

The next time you run `bin/up`, the {{ versions['toolkit-short'] }} will download and use the {{ versions['server-pro-short'] }} image.