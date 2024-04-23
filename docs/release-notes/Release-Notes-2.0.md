# Server Pro 2.7.1

Release date: 2021-09-16

Image ID: `823f6cb592f4` (Community Edition Image ID: `4f7d059759d2`)

### Bugfixes

- This release includes a security update preventing editor sessions from being disconnected.

### New Features

- An image placeholder is displayed when a template preview is not generated (due to having `ENABLE_CONVERSIONS` environment variable set to `false`).

# Server Pro 2.7.0

Release date: 2021-07-12

Image ID: `e8efe0e39698` (Community Edition Image ID: `9ca6a3256fb6`)

### Bugfixes
- Fixed comment attribution in projects with anonymous comments 
- Fixed collaborators redirected out of a project when the owner edited the collaborators
- Fixed redis connections in the host machine when no password authentication is defined [#895](https://github.com/overleaf/overleaf/pull/895)
- Fixed `host` headers in i18n installs [#896](https://github.com/overleaf/overleaf/pull/896)

### Other changes
- `SHARELATEX_SAML_CERT` [is now required](https://github.com/overleaf/overleaf/wiki/Server-Pro:-SAML-Config#passing-keys-and-certificates) for SAML setup.
- Many small improvements and bugfixes


# Server Pro 2.6.2

Release date: 2021-05-20

Image ID: `1f1f4f1c4f02` (Community Edition Image ID: `2d8a8b6d1930`)

### Bugfixes
- Fixed `references` service crashing with `.bib` files larger than 6mb.
- Fixed incorrect emailing attempt during user registration, which displayed an error message in the logs.
- Fixed LDAP registration when `firstName` and `lastName` contain a list of variations with non-ascii characters.


# Server Pro 2.6.1

Release date: 2021-04-23

Image ID: `dd382a10a3e5` (Community Edition Image ID: `cfd4ec616219`)

This release includes performance and stability improvements for handling larger projects. These improvements are backed by changes to data structures in the Mongo database. The changes are implemented in a backwards compatible way for reading existing data, but new data is written in a backwards incompatible format. You may be able to upgrade without making any changes to the database at this time, but in case of a roll-back, you will not be able to read all the newly written data. A future release will migrate the old data to the new format. As with previous updates, please ensure you have a backup before upgrading.

### Bugfixes
- This release includes important security updates. It fixes some potential XSS vulnerabilities and upgrades to node 12 ahead of node 10 reaching end of life on 30 Apr 2021.
- Fallback linearisation breaks PDF/A when not using sandboxed compiles.

### New Features
- SAML: Add `/saml/meta` endpoint to fetch [Service Provider Metadata](https://github.com/overleaf/overleaf/wiki/Server-Pro:-SAML-Config#metadata-for-the-identity-provider).
- [New configuration parameters](https://github.com/overleaf/overleaf/wiki/Configuring-SMTP-Email) for SMTP: `SHARELATEX_EMAIL_SMTP_NAME` and `SHARELATEX_EMAIL_SMTP_LOGGER`.
- Added [configurable AWS Region to email settings](https://github.com/overleaf/overleaf/wiki/Configuring-SMTP-Email) (`SHARELATEX_EMAIL_AWS_SES_REGION`).
- [Allow Nginx connection limits to be changed](https://github.com/overleaf/overleaf/wiki/HTTPS-reverse-proxy-using-Nginx).
- Administrators can now promote other users to administrator again.
- Added `ADDITIONAL_TEXT_EXTENSIONS` for customisation of [editable file types](https://github.com/overleaf/overleaf/wiki/Configuring-Overleaf).

### Other
- UI and performance improvements.
- All services have been upgraded to Node v12 from v10.

Note: `2.6.1` is the initial release on the `2.6.x` line, version `2.6.0` has not been published.

# Server Pro 2.5.2

Release date: 2021-01-26

Image ID: `8afec2bb0ace` (Community Edition Image ID: `62b08784fc5d`)

### Bugfixes

- Fixed account activation with uppercase emails (https://github.com/overleaf/overleaf/issues/819)

# Server Pro 2.5.1

Release date: 2021-01-14

Image ID: `e9a932e0effd` (Community Edition Image ID: `5d6493ce30fe`)

### Bugfixes
- Fixed log rotation

# Server Pro 2.5.0

Release date: 2020-11-20

Image ID: `7ae2382c68b3` (Community Edition Image ID: `6e6f3526af69`)

### New Features
- [File Outline](https://www.overleaf.com/blog/new-feature-file-outline-is-now-available-on-overleaf).
- [TexLive 2020](https://www.overleaf.com/blog/tex-live-2020-now-available) is now available.
- Mongo version updated to 4.0 (see [upgrade instructions](https://github.com/overleaf/overleaf/wiki/Updating-Mongo-version)).

### Bugfixes
- Fixed anonymous read/write project links redirecting to the login page.
- HTML code in SHARELATEX_LEFT_FOOTER is now correctly displayed.
- Fixed SHARELATEX_CUSTOM_EMAIL_FOOTER not working.

### Other 
- This will be the last CE/SP release where we officially support IE11. The next release in Q1 2021 will no longer be tested on IE11.

# Server Pro 2.4.2

Release date: 2020-10-5

Image ID: `83c76915b65a` (Community Edition Image ID: `814858a6bb38`)

### Bugfixes
- Fixed anonymous read/write sharing
- Fixed html links in left footer

# Server Pro 2.4.1

Release date: 2020-08-26

Image ID: `6cf8c22f2903` (Community Edition Image ID: `4be5fcb14878`)

No changes from 2.4.0. This release is just to keep the version numbering consistent between Server Pro and Community Edition.

# Server Pro 2.4.0

Release date: 2020-08-20

Image ID: `6cf8c22f2903` (Community Edition Image ID: `2ff144360052`)

### New Features
- Audit log for users' account settings updates

### Bugfixes
- This release fixes an important security issue: previously, it was possible for a specially crafted websocket request to bypass project authorization checks. https://github.com/overleaf/real-time/pull/177

### Other changes
- Many small improvements and bugfixes

# Server Pro 2.3.1

Release date: 2020-06-30

Image ID: `6fc8c7709df6` (Community Edition Image ID: `050f166647d2`)

### Bugfixes
- Fixed [sync between editor and pdf with synctex when not using sandboxed compiles](https://github.com/overleaf/overleaf/issues/756).

# Server Pro 2.3.0

Release date: 2020-06-11

Image ID: `feb1b349654b` (Community Edition Image ID: `843316001077`)

### New Features
- [TexLive version selector](https://www.overleaf.com/blog/new-feature-select-your-tex-live-compiler-version)
  - (when using sandboxed compiles)
- Show latexmk output in compile logs

### Bugfixes
- Fixed broken LuaLatex builds when not using sandboxed compiles
- Fixed PDF/A validation broken when not using sandboxed compiles
- Fixed project uploads from zip files with many large text files

### Other changes
- gravatar.com service is no longer used
- Deleted documents no longer count toward overall project file limit
- Improvements to dashboard tag management
- Many small improvements and bugfixes

# Server Pro 2.2.1

Release date: 2020-03-10

Image ID: `11dce4970997`

### Bugfixes

- TexLive image not being persisted in project entities (`project.imageName` field)

# Server Pro 2.2.0

Release date: 2020-02-10

Image ID: `4b4d9f8d4fad` (Community Edition Image ID: `0867310d1151`)

### Bugfixes

- Incorrect template layout is now fixed
- Node upgrade to fix CVE-2019-15605, CVE-2019-15606 and CVE-2019-15604

### New Features

- reCAPTCHA is now disabled
- Removed external access to Google Fonts.
- Other minor improvements and bugfixes.

# Server Pro 2.1.1

Release date: 2020-01-21

Image ID: `23f9a28eeef2` (Community Edition Image ID: `e01ae3fd29ac`)

### Bugfixes
- Fixed unable to share projects via email address.

# Server Pro 2.1.0

Release date: 2020-01-14

Image ID: `61a994c2584f` (Community Edition Image ID: `503763f211f2`)

### New Features
- Email confirmation banner can be disabled with the new environment variable `EMAIL_CONFIRMATION_DISABLED: 'true'`
- [New Archival/trashing workflow](https://www.overleaf.com/blog/new-feature-using-archive-and-trash-to-keep-your-projects-organized)
- Project ownership can be changed from the admin panel.

### Other Changes

- Admin: improved search using regular expressions 
- Other minor improvements and bugfixes.

# Server Pro 2.0.3

Release date: 2019-12-06

Image ID: `d29f5373eca9`

2.0.3 is a Server Pro-only release, no Community Edition has been published.

### Bugfixes
- Fixed unable to share projects via email address.

### Other Changes
- Reenabled recaptcha, which was the root cause of the issue above.

# Server Pro 2.0.2

Release date: 2019-11-26

Image ID: `9907ccc100e5` (Community Edition Image ID: `de647e8b462c`)

### Bugfixes
- Fixed link sharing for anonymous users when `SHARELATEX_ALLOW_PUBLIC_ACCESS: 'true'` and `SHARELATEX_ALLOW_ANONYMOUS_READ_AND_WRITE_SHARING: 'true'`.
- Fixed read-only access to shared projects.
- Fixed SAML logout (Server Pro only).
- Fixed sync between editor and pdf with `synctex` (Server Pro only).
- Fixed track changes slow to commit.

### Other Changes
- Disabled recaptcha (Server Pro only).
- Importing external files is disabled by default.
- **IMPORTANT**: A new `SYNCTEX_BIN_HOST_PATH` is required when using Sibling Containers. Check the setup instructions in the wiki: https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#mapping-the-location-of-synctex-in-the-host.

# Server Pro 2.0.1

Release date: 2019-10-18

Image ID: `cb014e03204a` (Community Edition Image ID: `708b20c0a403`)

### Bugfixes
- Fixed ["Delete Forever" button has no effect](https://github.com/overleaf/overleaf/issues/644).
- Fixed [admin creation using CLI](https://github.com/overleaf/overleaf/issues/647).

### Known issues
 - SyncTeX not working on Sandboxed Compiles [(see workaround instructions)](https://github.com/overleaf/overleaf/wiki/Fixing-SyncTeX-errors-in-Server-Pro-2.0.0-and-2.0.1).

# Server Pro 2.0.0

Release date: 2019-10-09

Image ID: `8fdd3419f5a5` (Community Edition Image ID: `2cdbdc229d4f`)

A lot has changed in the last few years, with ShareLaTeX [joining forces with Overleaf](https://www.overleaf.com/blog/518-exciting-news-sharelatex-is-joining-overleaf) to create one incredible authoring platform. Now that the merge is complete it's time to release a new version of ~ShareLaTeX~ Overleaf Community Edition and Server Pro!


## New Features

This release brings Community Edition and Server Pro up to date with the cloud-based version of [Overleaf](https://overleaf.com). A lot has changed since the last release, with too many bug fixes and small improvements to count, but here are a few highlights:

- A brand new user interface theme
- Improved project dashboard
- A new interface for creating (or uploading) files in a project
- Linked Files: import a file from another project.
- Improved History view


### Server Pro

This release of Server Pro includes a few new premium features:

- Rich Text mode: write your document like a rich-text document, backed by nice clean LaTeX
- Improved Sandboxed Compiles: no more fiddling with group permissions (see below)
- Improved Admin Interface: now you can inspect the state of a project from the Admin page, plus many more small improvements


## Changes to the Docker-Compose file format

This release adds a few new options to the docker environment. We recommend using our updated example [docker-compose.yml](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) file and updating it to reflect your old configuration where necessary. 

The new variables are:

- `ENABLED_LINKED_FILE_TYPES`: a comma-separated list-keys, which controls which types of "linked file" are available in the New File modal. Defaults to `'url,project_file'`, as in the example file.
- `ENABLE_CONVERSIONS`: If set to `'true'`, will enable on-the-fly file preview conversions using ImageMagick. Set to another value to disable this feature
- `REDIS_HOST`: should be set to the name of the Redis host, same as the `SHARELATEX_REDIS_HOST` variable. In the case of a simple docker-compose based deployment, this will just be `'redis'`. This duplication is, unfortunately, necessary in the short term while users migrate to the new Community Edition and Server Pro images, and will be resolved in a later release.
- `REDIS_PORT`: In case we desire to use the port for Redis other than the default, we need to set the value in `REDIS_PORT` to be the same as in `SHARELATEX_REDIS_HOST`, for the same reason described above.

And for Server Pro:

- `DOCKER_RUNNER`: set to `'true'` when enabling Sandboxed Compiles with sibling containers. See the updated documentation on [Sandboxed Compiles](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles).
- `SYNCTEX_BIN_HOST_PATH` (introduced in `2.0.2`): set to `<your_sharelatex_data_directory>/bin/synctex`. Check [Sibling Containers documentation](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#mapping-the-location-of-synctex-in-the-host) for extra context


## Changes to Sandboxed Compiles

We've made some changes to the way the [Sanboxed Compiles](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles) work. In previous versions, the administrators often needed to fiddle with user and group permissions on the docker socket to get Sibling Containers to work. In this release, we've changed all that so it's handled automatically inside the Server Pro container, so it should just work in the majority of cases.

From this release onwards we will no longer support the old "Docker-In-Docker" method of sandboxed compiles, as it has become more and more difficult to get this to work as time goes on. We strongly encourage admins to consider the newer Sibling Containers method as an alternative.


### Upgrading TexLive

You can now opt-in to a new version of TeX Live. The default is still TeX Live 2017, but you can find instructions on how to get TeX Live 2018 or later here: https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#changing-the-texlive-image

Before updating to a newer version of TexLive we strongly recommend [backing up your data](https://github.com/sharelatex/sharelatex/wiki/Backup-of-Data) and [update to the latest version of Server Pro available](https://github.com/overleaf/overleaf/wiki/Server-Pro:-setup#updating-image-versions).


### Database Migrations

The following database migrations have been added, and will run automatically:

- Alter the structure of the User model to support future work
- Change structure of `project.tokens`, to enforce uniqueness on the numeric part
  of a read-and-write sharing token

Administrators and end-users should not notice any change in behaviour due to these
migrations.


## Support

If anything seems out of place, or you run into any problems while upgrading, please
get in touch with us via email at `support@overleaf.com`
