From version `2.0.0` until `2.4.2` we haven't been too specific about which version of MongoDB should be used with {{ versions['community-edition-full'] }} and {{ versions['server-pro-short'] }}, with the exception for the minimum supported version specified in [supported dependencies list](/getting-started/software-requirements/#dependencies). 

Starting on `2.5.0`, any new release will indicate any change on the supported version of MongoDB in its [release notes](https://github.com/overleaf/overleaf/wiki#release-notes).


Similarly, the version of [`mongo` docker image](https://hub.docker.com/_/mongo) in [`docker-compose.yml`](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) has been historically untagged. Starting on {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} `2.5.0`, the tag will be updated to the version supported by the latest release of {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }}.

## Should I update MongoDB? ##

You should **only** consider updating your MongoDB version if you're planning to upgrade your instance of {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }}. 

If you're running a MongoDB version that is newer than the recommended for your current (or target) version (e.g, {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} 2.4.0 along with Mongo 4.2) there's no need to make any changes. **You should never downgrade your MongoDB version**.

If you experience a specific problem that you think might be related to your current version of MongoDB, feel free to [raise an issue](https://github.com/overleaf/overleaf/issues) if you are a {{ versions['community-edition-short'] }} user or contact Overleaf Support if you are {{ versions['server-pro-short'] }} a user.


### Checking your Mongo version ###

Opening the `mongo` shell should immediately print the current version.

{{ versions['toolkit-short'] }} users:
```
➤ bin/docker-compose exec mongo mongo -version
MongoDB shell version v4.4.1
```

Docker Compose users:
```
➤ docker compose exec mongo mongo -version
MongoDB shell version v4.4.1
```

## Update process ##

Updating the version of MongoDB during an upgrade of your {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} instance is as follows:

1. Decide the version of {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} you plan to upgrade to.
1. Find the version of MongoDB recommended by that specific Overleaf {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} release.
1. Follow the instructions to upgrade MongoDB to the target version.
1. Upgrade {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} image version and restart the instance.

Our recommendation is to always upgrade {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} **to the latest version available**, since it's always guaranteed to be supported ({{ versions['server-pro-short'] }} users only). In case you decide to go to an earlier version, this table shows the recommended version of MongoDB for earlier releases of {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }}, but you should never downgrade your MongoDB version.

| {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} Version   | MongoDB Version |
| --------------- |---------------| 
| 2.0.x           | 3.4           | 
| 2.1.x  to 2.4.x | 3.6           | 
| >=2.5.0         | 4.0           | 
| >=3.1.0         | 4.2           | 
| >=3.2.0         | 4.4           | 
| >4.2.0          | 5.0           |

### Upgrading MongoDB ###

MongoDB requires **step-by-step upgrades**. That means you can't go straight from, let's say `4.0` to `5.0`. You need first to update `4.2` to `4.4`, and then `5.0`.

!!! tip

    MongoDB uses even numbers for their stable versions.

**Update instructions when running mongo outside docker**

- [From `3.2` to `3.4`](https://docs.mongodb.com/manual/release-notes/3.4-upgrade-standalone)
- [From `3.4` to `3.6`](https://docs.mongodb.com/manual/release-notes/3.6-upgrade-standalone)
- [From `3.6` to `4.0`](https://docs.mongodb.com/manual/release-notes/4.0-upgrade-standalone)
- [From `4.0` to `4.2`](https://docs.mongodb.com/manual/release-notes/4.2-upgrade-standalone)
- [From `4.2` to `4.4`](https://docs.mongodb.com/manual/release-notes/4.4-upgrade-standalone)
- [From `4.4` to `5.0`](https://www.mongodb.com/docs/manual/release-notes/5.0-upgrade-replica-set/)

Note that the instructions for `5.0` point to a replica set install, instead of standalone. MongoDB needs to be run as a replica set since [Server Pro 4.0.1](/release-notes/Release-Notes-4.x.x/#server-pro-421).

**Docker users**
- Update the version of the `mongo` image (docker-compose setup: edit `services -> mongo -> image`; {{ versions['toolkit-short'] }} setup: update `MONGO_IMAGE`, e.g. `MONGO_IMAGE=mongo:5.0`)

In most cases the update just requires setting up a compatibility setting before actually updating the version. Let's see an example.

### Example: Upgrading MongoDB from `4.4` to `5.0` ###

Let's start by making sure we're running MongoDB `4.4`: 

{{ versions['toolkit-short'] }} users:
```
➤ bin/docker-compose exec mongo mongo -version
MongoDB shell version v3.4.24
```

Docker Compose users:
```
➤ docker compose exec mongo mongo -version
MongoDB shell version v3.4.24
```

According to the [upgrade instructions](https://docs.mongodb.com/manual/release-notes/3.6-upgrade-standalone/#upgrade-version-path), the only requirement is to have `featureCompatibilityVersion` set to `4.4`. We do so by opening a mongo shell and running the indicated command:

{{ versions['toolkit-short'] }} users:
```
➤ bin/docker-compose exec mongo mongo
MongoDB shell version v3.4.24
...
> db.adminCommand( { setFeatureCompatibilityVersion: "4.4" } )
{ "ok" : 1 }
> exit
bye
```

Docker Compose users:
```
➤ docker-compose exec mongo mongo
MongoDB shell version v3.4.24
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.24
Welcome to the MongoDB shell.
For interactive help, type "help".
> db.adminCommand( { setFeatureCompatibilityVersion: "4.4" } )
{ "ok" : 1 }
> exit
bye
```

{{ versions['toolkit-short'] }} users:

We'll then stop {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} and MongoDB instances using the `bin/stop` command, set `MONGO_IMAGE=5.0` in `config/overleaf.rc`, and then restart the `mongo` service using `bin/up mongo`) to verify the update went smoothly.

Finally, we'll update the {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} image version to our target version and restart all the services using the `bin/up -d` command.

Docker Compose:

We'll then stop {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} and MongoDB instances using the `docker compose down` command, update [`docker-compose.yml`](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) file to use `image: mongo:5.0`, and then restart the `mongo` service using the `docker compose up mongo` command to verify the update went smoothly.

Finally, we'll update {{ versions['community-edition-short'] }}/{{ versions['server-pro-short'] }} image version to our target version and restart all the services using the `docker compose up` command.