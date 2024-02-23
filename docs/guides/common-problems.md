In this section, we aim to address frequently encountered issues that you may encounter while running your own on-premises version of Overleaf. 

## CLSI ##

Log location in the container: `/var/log/sharelatex/clsi.log`

### `(HTTP code 400) unexpected - OCI runtime create failed` ###

**Snippet:** `...opt/synctex` ... `not a directory`

Full message reads roughly (after removing quite a lot of noise):

> <path\> is not a directory; Are you trying to mount a directory onto a file (or vice-versa)? Check if the specified host path exists and is the expected type 

Check the value of the `SYNCTEX_BIN_HOST_PATH` environment variable. For more information, please see [this documentation](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#mapping-the-location-of-synctex-in-the-host).

## Running Overleaf with an NFS filesystem ##

Mounting a NFS filesystem in an Overleaf container is technically possible, but it's not recommended and can result in different types of performance errors.

One common error that compiles see is:
> EBUSY: resource busy or locked, unlink '/var/lib/sharelatex/data/compiles/62f3d57bef7cf9005c364e75-62f3d57bef7cf9005c364e7a/.nfs573663533034825247625441'

In particular we advise against using NFS backed filesystems for ephemeral data, like the directories use for compilation data. We recommend using a local scratch disk, preferably a local SSD for the following directories:

| Path | Description |
|------|-------------|
|`/var/lib/sharelatex/tmp` |ephemeral data such as uploads and processing zip file data, `Settings.path.dumpFolder` and `Settings.path.uploadFolder` are sub-directories of this folder |
| `/var/lib/sharelatex/data/cache` |cache for binary files of projects that are being compiled (`Settings.path.clsiCacheDir`) |
| `/var/lib/sharelatex/data/compiles` |ephemeral compile dir data (`Settings.path.compilesDir`)|
| `/var/lib/sharelatex/data/output` |ephemeral compile artifacts for download by the browser (`Settings.path.outputDir`)|

For `docker-compose` based setups, we suggest just overriding the bind-mount from NFS, which avoids changing paths in the application. Here's an example of a `docker-compose` config excerpt with the use of a scratch disk that is mounted at `/scratch`:

```yaml
services:
  sharelatex:
    environment:
      SANDBOXED_COMPILES_HOST_DIR: /scratch/compiles/
    volumes:
      - nfs:/var/lib/sharelatex/data
      - /scratch/cache/:/var/lib/sharelatex/data/cache
      - /scratch/compiles/:/var/lib/sharelatex/data/compiles
      - /scratch/output/:/var/lib/sharelatex/data/output
      - /scratch/tmp/:/var/lib/sharelatex/tmp
```

There is no need to migrate any existing files from the NFS to their new home after the update. The LaTeX compiler can recreate all the files with a full compilation run again.