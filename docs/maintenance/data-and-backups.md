Some times we need to change the schema of data in the database as we evolve Overleaf, migration scripts are used to automate this process. They will have been run on overleaf.com first which is the largest instance of Overleaf in the world so most eventualities will have been encounter already, however we make no guarantees over your data. Please ensure that you create a consistent back of your data before upgrading you instance.

## Data storage ##

{{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }} store their data in three separate places:

* **Mongo Database:** This is where user and project data reside.
* **Redis:** serves as a high-performance cache for in-flight data, primarily storing information related to project editions and collaboration.
* **Overleaf Filesystem:** stores non-editable project files (including images) and also acts as a temporary disk cache during project compilations.
   
    !!! note
     
         This might be `~/sharelatex_data` or `~/overleaf_data`, depending on when you're instance was setup.
    
    
    !!! note
    
         For project files and full project history data we also support [S3 compatible storage backends](/configuration/s3).

See [section "Folders in detail" for details](#Folders-in-detail) on the folder layout on disk.

## Performing a consistent backup ##

There are three stores which need to included when taking a consistent backup:

* MongoDB
* Redis
* Overleaf Filesystem data

In order to produce a consistent backup it is **mandatory** to stop users from producing new data while the backup process is running. We therefore advise scheduling a maintenance window during which, users should not be able to access the instance or edit their projects.

Before you start the backup process you will need to take your instance offline. Starting with Server Pro `3.5.0` the shutdown down process automates the closing of the site and the disconnection of users.

To shutdown your instance you'll need to run `bin/docker-compose stop sharelatex` if you are running a {{ versions['toolkit-short'] }} deployment or `docker compose stop sharelatex` if you are running Docker Compose.

Once the `sharelatex` container has been stopped you and start the backup process.

Once the backup process has been completed successfully you'll need to start the `sharelatex` container. To do this run `bin/docker-compose start sharelatex` if you are running a {{ versions['toolkit-short'] }} deployment or `docker compose start sharelatex` if you are running Docker Compose.

!!! important

    - Backups should be stored on a separate server to the one your Overleaf instance is running on, ideally in a different location entirely. 
    
    - Replicating databases onto multiple MongoDB instances might offer some redundancy, it doesn't safeguard against corruption.
    
    - Testing your backups is the best way to ensure they are complete and functional.

## MongoDB ##

MongoDB comes with a command-line tool called [mongodump](https://docs.mongodb.com/manual/reference/program/mongodump/) which can be used to create a backup of user and project data stored in the database.

## Overleaf Filesystem data ##

For {{ versions['toolkit-short'] }} deployments, the path where your non-editable files are stored is specified in `config/overleaf.rc` using the `OVERLEAF_DATA_PATH` environment variable. This might be: `data/sharelatex`, using a tool such as **rsync** to recursively copy this directory is required to ensure a complete backup is created.

## Redis ##

Redis stores user sessions and pending document updates before they are flushed to MongoDB. 

### AOF (Append Only File) ###

AOF (Append Only File) persistence is the recommended configuration for Redis persistence.

[{{ versions['toolkit-short'] }}](https://github.com/overleaf/toolkit) users have AOF persistence enabled by default for new installs. Existing users are recommended to follow the instructions on the official documentation to switch to AOF:

[https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/#how-i-can-switch-to-aof-if-im-currently-using-dumprdb-snapshots](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/#how-i-can-switch-to-aof-if-im-currently-using-dumprdb-snapshots)

If you decide to continue using RDB snapshots along with AOF persistence you can copy the RDB file to a secure location as a backup.

## Migrating data between servers ##

At best you do not have any valuable data in the new instance yet. We do not have a process for merging the data of instances.

Assuming the new instance has not data yet, here are some steps you could follow.
On a high level we produce a tar-ball of the mongo, redis and overleaf volumes, copy it over to the new server and inflate it there again.

With the default docker-compose file that would be:
```
# Gracefully shutdown the old instance
# (See https://github.com/overleaf/overleaf/wiki/Data-and-Backups#performing-a-backup)
old-server$ docker stop sharelatex
old-server$ docker stop mongo redis

# Create the tar-ball
old-server$ tar --create --file backup-old-server.tar ~/OVERLEAF_data ~/mongo_data ~/redis_data

# Copy the backup-old-server.tar file from the old-server to the new-server using any method that fits

# Gracefully shutdown new instance (if started yet)
new-server$ docker stop sharelatex
new-server$ docker stop mongo redis

# Move new data, you can delete it too
new-server$ mkdir backup-new-server
new-server$ mv ~/OVERLEAF_data ~/mongo_data ~/redis_data backup-new-server/

# Populate data dirs again
new-server$ tar --extract --file backup-old-server.tar

# Start containers
new-server$ docker start mongo redis
new-server$ docker start sharelatex
```
Depending on your docker-compose config, you may need to adjust the paths of the mongo/redis/overleaf volume.


With a toolkit setup that would be:
```
# Gracefully shutdown the old instance
# (See https://github.com/overleaf/overleaf/wiki/Data-and-Backups#performing-a-backup)
old-server$ bin/stop

# Create the tar-ball
old-server$ tar --create --file backup-old-server.tar config/ data/

# Copy the backup-old-server.tar file from the old-server to the new-server using any method that fits

# Gracefully shutdown new instance (if started yet)
new-server$ bin/stop

# Move new data, you can delete it too
new-server$ mkdir backup-new-server
new-server$ mv config/ data/ backup-new-server/

# Populate config/data dir again
new-server$ tar --extract --file backup-old-server.tar

# Start containers
new-server$ bin/up
```
## Folders in detail ##

1. `~/mongo_data` (b)
   - mongodb datadir
2. `~/redis_data` (b)
   - redis db datadir
3. `~/overleaf_data`
   1. bin
      1. synctex (d)
         - unused in latest release, previously a custom synctex binary was used
           (synctex is used for source mapping between .tex files and the pdf) 
   2. data
      1. cache (e)
         - binary file cache for compiles
      2. compiles (e)
         - latex compilation happens here
      3. db.sqlite (d)
         - unused in latest release, previously stored clsi cache details
           (either moved to simple in-memory maps or we scan the disk)
      4. db.sqlite-wal (d)
         - unused in latest release, see db.sqlite
      5. output (e)
         - latex compilation output storage for serving to client
      6. template_files (b)
         - image previews of template system (Server Pro only)
      7. user_files (b)
         - binary files of projects
      7. history (b)
         - full project history files
   3. tmp
      1. dumpFolder (e)
         - temporary files from handling zip files
      2. uploads (e)
         - buffering of file uploads (binary file/new-project-from-zip upload)
      2. projectHistories (e)
         - temporary files for full project history migrations

The folders have additional hints:
- (b) include in backups, best when the instance is stopped to ensure consistency
- (d) can be deleted
- (e) ephemeral files, can be deleted when the instance is stopped
