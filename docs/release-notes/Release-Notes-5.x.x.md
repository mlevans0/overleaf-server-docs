## Server Pro 5.0.1 **(Retracted)** ##

!!! warning

    2024-04-18: We have identified a critical bug in a database migration that causes data loss. Please defer upgrading to release 5.0.1 until further notice on the mailing list. 
    
    **Please hold on to any backups that were taken prior to upgrading to version 5.0.1.**

Release date: 2024-04-02

- Server Pro Image ID: `0d28770b4692`
- Community Edition Image ID: `ee69bf0baddf`
- Git Bridge Image ID: `455a8c0559a4`

!!! note

    An issue was discovered with version `5.0.0`, so it was never made public. This resulted in `5.0.1` being the first release in the `5.0` release line.

This major release includes the following changes:

- Required database upgrade from MongoDB 4 to MongoDB 5
- Rebranding of `SHARELATEX_*` to `OVERLEAF_*` environment variables
- Rebranding of filesystem paths from ShareLaTeX brand to Overleaf brand

**Important**: the [Overleaf Toolkit](https://github.com/overleaf/toolkit) will help migrating your configuration, please follow the prompts of `bin/upgrade`.

### MongoDB upgrade to v5 ###

MongoDB 4.4 has reached end of life on February 2024. All customers should [upgrade to MongoDB 5.0](https://github.com/overleaf/overleaf/wiki/Updating-Mongo-version) before upgrading to the 5.0 release line.

The release also includes migrations that update the database in a backwards incompatible format. 

Please ensure you have a [database backup](https://github.com/overleaf/overleaf/wiki/Data-and-Backups) before upgrading. In case of roll-back, you will need to restore the database backup. Server Pro 4.x is not capable of reading the new format, which can result in data-loss or broken projects.

### Configuration changes ###

Environment variables have been rebranded from `SHARELATEX_*` to `OVERLEAF_*`. [Overleaf Toolkit](https://github.com/overleaf/toolkit) users should be prompted to perform the migration when running `bin/upgrade`, and warnings will be printed when trying to run the Overleaf instance with the incorrect configuration.

Filesystem paths have also been rebranded from ShareLaTeX brand to Overleaf brand:
- `/var/lib/sharelatex` -> `/var/lib/overleaf`
- `/var/log/sharelatex` -> `/var/log/overleaf`
- `/etc/sharelatex` -> `/etc/overleaf`

Filesystem changes are automatically handled by the Overleaf Toolkit. Otherwise, make sure bind-mount targets are updated to refer to the Overleaf equivalent, e.g.

`docker-compose/yml` before:

```yml
    volumes:
     - /my/docker-host/path:/var/log/sharelatex
```

`docker-compose.yml` after:

```yml
    volumes:
     - /my/docker-host/path:/var/log/overleaf
```

### New Features ###

- Added support for using IAM credentials when using AWS S3 for project/history files
- Server Pro will refuse to start when using an older version of MongoDB

### Bugfixes ###

- Fixes a scenario in which the share project modal doesn't display the link-sharing links immediately after turning on the feature

### Other changes ###

- All services are now using IPv4 in the container
- Container image upgrade from Ubuntu 20.04 to 22.04 LTS
- Security updates to the base image and installed packages, along with improvements and bugfixes.