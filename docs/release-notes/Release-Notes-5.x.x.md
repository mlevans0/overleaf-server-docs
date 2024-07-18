## Server Pro  5.1.0 ##

Release date: 2024-07-17

- Server Pro Image ID: `7216db608356`
- Community Edition Image ID: `41a77f59f69e`
- Git Bridge Image ID: `4cd4bea6fb01`

### MongoDB upgrade to v6 ###

MongoDB 5 is reaching end of life on [October 2024](https://www.mongodb.com/legal/support-policy/lifecycles). All customers should upgrade to MongoDB 6.0. [Follow the link to the official documentation for instructions](https://github.com/overleaf/overleaf/wiki/Updating-Mongo-version).

{{ versions['toolkit-short'] }} users now need to split the MongoDB image between `MONGO_IMAGE` (with just the image name) and `MONGO_VERSION` in their `config/overleaf.rc` file.

Example:

```
# When using a custom image, MONGO_VERSION is required
MONGO_IMAGE=my.docker.hub/mongo
MONGO_VERSION=6.0-custom
```

Please ensure you have a [consistent database backup](/maintenance/data-and-backups/) before upgrading.

### Redis AOF Persistence enabled by default ###

AOF (Append Only File) persistence is now the recommended configuration for Redis persistence.

{{ versions['toolkit-short'] }} users have AOF persistence enabled by default for new installs. Existing users are recommended to follow the instructions on the official documentation to switch to AOF: [https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/#how-i-can-switch-to-aof-if-im-currently-using-dumprdb-snapshots](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/#how-i-can-switch-to-aof-if-im-currently-using-dumprdb-snapshots)

### Deprecation for docker-compose v1 ###

`docker-compose` v1 has reached its End Of Life in July 2023 (https://docs.docker.com/compose/migrate/). Support for `docker-compose` v1 in the Overleaf {{ versions['toolkit-short'] }} will be dropped with the release of Server Pro 5.2. We recommend upgrading to Docker Compose v2 before then.

### New Features ###

- SAML: multiple certificates are now supported. You can now set a list of comma-separated certificates in `OVERLEAF_SAML_SIGNING_CERT` and `OVERLEAF_SAML_CERT`
- CSP (Content Security Policy) is now enabled by default. 

### Bugfixes ###

- Fixes a bug where projects created before enabling the templates feature couldn't be published as templates.
- Fixed spacing in project list footer.
- Fixed post-login redirection when login after clicking the "Log in" button in the header. 

### Other changes ###

- Removed support for running LaTeX compiles with Docker-In-Docker in Server Pro. Sandboxed compiles using "sibling" containers is not affected by this.
- TeXLive images, as used for Sandboxed compiles, need to be pulled outside of Server Pro now. The Overleaf {{ versions['toolkit-short'] }} is pulling all configured images as part of `bin/up`. All customers have been granted read access to quay.io/sharelatex/texlive-full.
- Stricter and faster graceful shutdown procedure for the Server Pro container
- The environment variable `SYNCTEX_BIN_HOST_PATH` is no longer used by the application
- We are sunsetting window properties like `window.project_id`. If you need access to any of these, please reach out to [support@overleaf.com](mailto:support@overleaf.com) to discuss options.
- Significant reduction in Docker image size for Server Pro and CE
- Security updates to the base image and installed dependencies.
- Minor improvements and bugfixes. 


## Server Pro 5.0.7 ##

Release date: 2024-07-12

- Server Pro Image ID: `a8c301474a4d`
- Community Edition Image ID: `6f3e55a67fd5`
- Git Bridge Image ID: `455a8c0559a4 `

This is a security release. We added stricter controls for accessing project invite details and locked down access to files via the LaTeX compilation service.

We strongly recommend turning on the [Sandboxed compiles feature](https://github.com/overleaf/overleaf/wiki/Server-Pro:-Sandboxed-Compiles) in Server Pro.

## Server Pro 5.0.6 ##

Release date: 2024-06-20

- Server Pro Image ID: `c9de60b06959`
- Community Edition Image ID: `46bb44d4215d`
- Git Bridge Image ID: `455a8c0559a4`

This is a security release. We added stricter controls for creating projects from ZIP URLs.

## Server Pro 5.0.5 ##

Release date: 2024-06-11

- Server Pro Image ID: `60da5806f83e`
- Community Edition Image ID: `46bb44d4215d`
- Git Bridge Image ID: `455a8c0559a4`

This is a security release. We added stricter controls to prevent arbitrary CSS loading in the project editor.

## Server Pro 5.0.4 ##

Release date: 2024-05-24

- Server Pro Image ID: `b0db0405a7ce`
- Community Edition Image ID: `abcec6efbbf7`
- Git Bridge Image ID: `455a8c0559a4`

This release provides security updates, bug fixes, and performance enhancements, including:

- Stricter controls to prevent arbitrary JavaScript execution in the browser.
- Updated libraries to enhance security and performance.

## Server Pro 5.0.3 ##

Release date: 2024-04-24

- Server Pro Image ID: `dc88a9ade14d`
- Community Edition Image ID: `b4712d596c75`
- Git Bridge Image ID: `455a8c0559a4`

This release builds up on 5.0.2 and includes the second revision of the recovery process for doc versions.

If you never ran Server Pro version 5.0.1 or Community Edition version 5.0.1, or you started a brand new instance with 5.0.1, you do not need to run this recovery process. Please see the [Bugfixes section for Server Pro 5.0.2 below for details on the need for a recovery](#bugfixes) and follow the [doc version recovery process](/guides/doc-version-recovery/).

## Server Pro 5.0.2 **(Retracted)** ##

!!! warning

    2024-04-22: We are retracting version 5.0.2. We have identified a few corner cases in the recovery procedure for docs.

    2024-04-24: Server Pro version 5.0.3 sports fixes for the previously identified corner cases.

Release date: 2024-04-22

- Server Pro Image ID: `06eed5680340`
- Community Edition Image ID: `9f018f899ba5`
- Git Bridge Image ID: `455a8c0559a4`

### Security release ###

**Server Pro 5.0.2 is a security release for the application runtime.**

The Node.js runtime has been upgraded to `18.20.2`. Check their release notes ([`18.20.1`](https://nodejs.org/en/blog/release/v18.20.1), [`18.20.2`](https://nodejs.org/en/blog/release/v18.20.2)) for more information.

### Bugfixes ###

- Fixes database migration that resulted in the loss of doc versions. These are used by the history system and their loss resulted in the history system skipping over updates effectively resulting in no further changes to the history view and git-integration. This release fixes the database migration and also sports a recovery process for instances that ran release 5.0.1. If you ran version 5.0.1, please take a look at the dedicated [doc version recovery process](/guides/doc-version-recovery/).
- Fixes `references` and `templates` services on Docker 26  ipv6.

### Other changes ###

- Adds `bin/flush-history-queues` and `bin/force-history-resyncs` utility scripts.

## Server Pro 5.0.1 **(Retracted)** ##

!!! warning

    2024-04-18: We have identified a critical bug in a database migration that causes data loss. Please defer upgrading to release 5.0.1 until further notice on the mailing list. 
    
    2024-04-24: Server Pro 5.0.3 has been released with a fix and recovery process that does not need access to a backup. See details above.

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

**Important**: the [{{ versions['toolkit-short'] }}](https://github.com/overleaf/toolkit) will help migrating your configuration, please follow the prompts of `bin/upgrade`.

### MongoDB upgrade to v5 ###

MongoDB 4.4 has reached end of life on February 2024. All customers should [upgrade to MongoDB 5.0](https://github.com/overleaf/overleaf/wiki/Updating-Mongo-version) before upgrading to the 5.0 release line.

The release also includes migrations that update the database in a backwards incompatible format. 

Please ensure you have a [consistent database backup](/maintenance/data-and-backups/) before upgrading. In case of roll-back, you will need to restore the database backup. Server Pro 4.x is not capable of reading the new format, which can result in data-loss or broken projects.

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