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