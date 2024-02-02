## Overview ##

{{versions['community-edition-short']}} and {{versions['server-pro-short']}} are both complex systems that are made up of many different services and processes that all generate logs that ares stored in `/var/log/sharelatex/`, where they can be monitored and analyzed for troubleshooting, security purposes, and understanding system activity.

If an error occurs in any of the processes it will be written to the respective log file such as `/var/log/sharelatex/web.log`.

## {{ versions['toolkit-full'] }} users ##

{{ versions['toolkit-full'] }} users can have a look at the logs inside the container using the `bin/logs` script:

```
$ bin/logs web

# You can use --help for help
$ bin/logs --help

# You can also look at the logs for multiple services at once:
$ bin/logs filestore docstore web clsi

# You can follow the log output using the -f flag
$ bin/logs -f filestore docstore web clsi

# You can use the -n {number} flag to limit the number of lines to print (default 50)
$ bin/logs -n 50 web

# You can use the -n all flag to show all log lines
$ bin/logs -n all web

# You can use > to redirect the output to a file
$ bin/logs -n all web > web.log
```
You can use the `bin/logs` script to view logs for the following services: `clsi`, `contacts`, `docstore`, `document-updater`, `filestore`, `git-bridge`, `mongo`, `notifications`, `real-time`, `redis`, `spelling`, `tags`, `track-changes`, `web`, `history-v1`, `project-history`.

## Copying logs ##

You can copy log files from the main `sharelatex` container to local computer using the following command:

```
docker cp sharelatex:/var/log/sharelatex/{server-name}.log {service-name}.log
```

## Tracking project access ##

It is possible to see who and when a project is loaded with the following command:

```
docker exec sharelatex bash -c "grep "join project request" /var/log/sharelatex/web.log"
```


This will give both the `timestamp`, `user_id` and `project_id`.