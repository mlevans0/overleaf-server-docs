!!! note

    These instructions are for v5.x and later. If you are following this guide for an earlier version please use `sharelatex` instead of `overleaf` in path names.

This guide will walk you through the migration from on-disk storage to an S3 compatible object storage.

This document refers to sections of the introduction document on the [S3 Setup](https://github.com/overleaf/overleaf/wiki/S3).

!!! note "We'd ove to hear from you"

    If you'd like to share with us how many files you migrated over, their overall volume and how long the migration took - we've love to hear from you. [Please drop us a message at `support@overleaf.com`](mailto:support@overleaf.com) .

## Requirements ##

- A S3 compatible object storage to talk to, see "[S3 Setup](https://github.com/overleaf/overleaf/wiki/S3)" for options
- Free disk space for migrating existing data, about the current on disk size
- A maintenance window for doing the actual migration
- A full backup, including the config, to enable restoring from it

## Estimate the disk size needed for the migration ##

We can use `du(1)` for calculating the current disk usage:
```shell
docker exec sharelatex \
  du --human-readable --max-depth=0 /var/lib/overleaf/data/user_files

docker exec sharelatex \
  du --human-readable --max-depth=0 /var/lib/overleaf/data/template_files
```

In case you do not have sufficient disk space available on the current server, try attaching another disk to the server.

!!! note

    The history directories already have the correct layout. You can upload directly from the bind-mounted source folder, which does not require any additional disk space.

## Migration steps ##

### Step 0 shutdown the instance ###

We need to make sure that all user/template files will get migrated. It is best to shut down the instance to avoid missing newly uploaded files.

Please see our guide on [performing a consistent a backup](/maintenance/data-and-backups#performing-a-consistent-backup) for the shutdown procedure.

### Step 1 rewrite directory layout ###

We need to rewrite the directory layout of project files for uploading them to S3.The directory layout for local storage in filestore is `<project-id>_<file-id>` and the directory layout in S3 is `<project-id>/<file-id>`.

In the following, `/srv/overleaf-s3-migration` is used for storing the files in the new directory layout.

We can make use of `tar(1)` for rewriting the layout:

```shell
mkdir -p /srv/overleaf-s3-migration/user_files \
         /srv/overleaf-s3-migration/template_files
docker exec sharelatex \
  tar --create --directory /var/lib/overleaf/data/user_files . \
| tar --extract --directory /srv/overleaf-s3-migration/user_files \
  --transform=sx_x/x
docker exec sharelatex \
  tar --create --directory /var/lib/overleaf/data/template_files . \
| tar --extract --directory /srv/overleaf-s3-migration/template_files \
  --transform=sx_x/xg
```

### Step 2 upload the files ###

Depending on your preference, you can use the minio mc S3 client or the aws cli for uploading the files to your S3 compatible object storage.

#### aws cli ####

!!! note

    Here you should replace `overleaf-user-files`, `overleaf-template-files`, `overleaf-project-blobs` and `overleaf-chunks` with the names of your S3 buckets.

!!! note

    Also replace `/srv/overleaf-bind-mount` with the local path of the `/var/lib/overleaf` bind-mount. By default, this is `~/overleaf_data` in a docker-compose.yml deployment and `<toolkit-checkout>/data/overleaf` when using the {{ versions['toolkit-short'] }}.

```shell
aws s3 sync /srv/overleaf-s3-migration/user_files s3://overleaf-user-files
aws s3 sync /srv/overleaf-s3-migration/template_files s3://overleaf-template-files

aws s3 sync /srv/overleaf-bind-mount/data/history/overleaf-project-blobs s3://overleaf-project-blobs
aws s3 sync /srv/overleaf-bind-mount/data/history/overleaf-chunks s3://overleaf-chunks
```

#### minio mc ###

We are using the server alias "s3" here, you may have picked another name.

```shell
mc mirror /srv/overleaf-s3-migration/user_files s3/overleaf-user-files
mc mirror /srv/overleaf-s3-migration/template_files s3/overleaf-template-files

mc mirror /srv/overleaf-bind-mount/data/history/overleaf-project-blobs s3/overleaf-project-blobs
mc mirror /srv/overleaf-bind-mount/data/history/overleaf-chunks s3/overleaf-chunks
```

### Step 3 start the instance pointing at S3 ###

Add all the S3 related variables to your config, as detailed in the "[S3 Setup -> Overview of variables](https://github.com/overleaf/overleaf/wiki/S3#overview-of-variables)" section.

For Docker Compose deployments, you can also remove the bind-mount for the data dir from the volumes section.

!!! note

    Please keep the [bind-mount of a scratch disk for ephemeral files](https://github.com/overleaf/overleaf/wiki/Troubleshooting#running-overleaf-with-a-nfs-filesystem) in place.

You can now start the instance and validate the migration:
- can preview binary files in the editor
- can compile a PDF with images
- can upload new files

## Rolling back ##

You can roll back the migration gracefully in reversing the steps:

1. shutdown the instance
2. mirror back the files by flipping the sequence of source/destination
3. write new files back into the local directory using an inverse `transform`
4. restart the instance with the old configuration


```shell
# When using aws cli
aws s3 sync s3://overleaf-user-files /srv/overleaf-s3-migration/user_files
aws s3 sync s3://overleaf-template-files /srv/overleaf-s3-migration/template_files
aws s3 sync s3://overleaf-project-blobs /srv/overleaf-bind-mount/data/history/overleaf-project-blobs
aws s3 sync s3://overleaf-chunks /srv/overleaf-bind-mount/data/history/overleaf-chunks

# When using minio mc
mc mirror s3/overleaf-user-files /srv/overleaf-s3-migration/user_files
mc mirror s3/overleaf-template-files /srv/overleaf-s3-migration/template_files
mc mirror s3/overleaf-project-blobs /srv/overleaf-bind-mount/data/history/overleaf-project-blobs
mc mirror s3/overleaf-chunks /srv/overleaf-bind-mount/data/history/overleaf-chunks
```

```shell
# Write files into local Server CE/Server Pro
tar --create --directory /srv/overleaf-s3-migration/user_files . \
| docker exec --interactive sharelatex \
    tar \
      --extract \
      --keep-old-files \
      --directory /var/lib/overleaf/data/user_files \
      --transform=sx./xx --transform=sx/x_x \
      --wildcards '*/*/*'

tar --create --directory /srv/overleaf-s3-migration/template_files . \
| docker exec --interactive sharelatex \
    tar \
      --extract \
      --keep-old-files \
      --directory /var/lib/overleaf/data/template_files \
      --transform=sx./xx --transform=sx/x_xg \
      --wildcards '*/*/*/*/pdf-converted-cache/*' \
      --wildcards '*/*/*/*/pdf' \
      --wildcards '*/*/*/*/zip'
```

!!! note

    The first transform removes the top level folder. The 2nd transform changes the directory layout to a flat one. The wildcards ensure that only files are extracted, not their parent (project) folders.