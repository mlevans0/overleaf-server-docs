## Consulting the Doctor ##

The {{ versions['toolkit-full'] }} comes with a handy tool for debugging your installation: `bin/doctor`

Let's run the `bin/doctor` script:

```sh
$ bin/doctor
```

We should see some output similar to this:

```
====== Overleaf Doctor ======
- Host Information
    - Linux
    ...
- Dependencies
    - bash
        - status: present
        - version info: 5.1.16(1)-release
    - docker
        - status: present
        - version info: Docker version 25.0.3, build 4debf41
    - docker compose
        - status: present
        - version info: docker compose version v2.25.5
    ...
====== Configuration ======
    ...
====== Warnings ======
- None, all good
====== End ======
```

First, we see some information about the host system (the machine that the {{ versions['toolkit-short'] }} is being run on), then some information about dependencies. If any dependencies are missing, we will see a warning here. Next, the doctor checks our local configuration. At the end, the doctor will print out some warnings, if any problems were encountered.

When you run into problems with your {{ versions['toolkit-short'] }}, you should first run the doctor script and check it's output. 

## Validating the version number ##

In certain setups, particularly when customers are overriding configuration defaults, re-tagging images, or hosting their repositories, it may be sometimes necessary to validate the version of the Overleaf you are running, especially when minor changes have been made and the differences might not be so noticeable. You can validate the version number by running the following command and then checking the our release notes](https://github.com/overleaf/overleaf/wiki#release-notes) for the version that corresponds to the returned value.

```
# Get the first 12 characters of Image Id from the sharelatex image
$ docker inspect --format '{{ verbatim_strings[".Image"] }}' sharelatex | cut -d ':' -f 2 | cut -c 1-12

3a75a815d297
```
In this case, the truncated Image ID `3a75a815d297` is for [Server Pro 4.2.1](https://github.com/overleaf/overleaf/wiki/Release-Notes--4.x.x#server-pro-421)

## Debug logging ##

Sometimes it might be necessary to enable additional logging so that we have more visibility of what happening. To enable debug logging for specific period of time (in this case 1-day), and without introducing any downtime, please run the following command from your {{ versions['server-pro-short'] }} instance host. 

```
docker exec sharelatex bash -exc 'mkdir -p /logging && echo $(node -p "Date.now()+1000*60*60*24") > /logging/tracingEndTime'
```

!!! note

    Logging will be in debug mode for 1-day but the debug logging event trigger file will remain in place until it is deleted.

Once you've run the command it'll take up to 1 minute for {{ versions['server-pro-short'] }} to start logging in debug mode.

To stop debug logging before 1-day, or to remove the debug logging event trigger file, please run the following command from your {{ versions['server-pro-short'] }} instance terminal. 

```
docker exec sharelatex rm /logging/tracingEndTime
```

## Getting help

!!! important

    Users of the free {{ versions['community-edition-full'] }} should [open an issue on github](https://github.com/overleaf/toolkit/issues) and include the output of the `bin/doctor` script in your message. 
    
    Users of {{ versions['server-pro-full'] }} should contact `support+serverpro@overleaf.com` for assistance. 

When reaching out, to help us identify what the issue might be, please provide us with either a copy of your **docker-compose.yml** file (ensuring that all sensitive information is redacted ) or the output from running `bin/doctor` and the contents of the **/config** folder (again redacting all sensitive information) if you are using the {{ versions['toolkit-short'] }}. 

In addition to that, please also let us know the answers to the following:

- What version of {{ versions['server-pro-short'] }} are you currently running?
- What hardware and operating system are you using?
- Have any recent changes been made to your {{ versions['server-pro-short'] }} instance, for example, configuration changes made to {{ versions['server-pro-short'] }} itself, the host or any hardware/infrastructure changes?

Please also provide us with a copy of the **web.log**. You can get a copy of this log from the container by running this command: 
`docker cp sharelatex:/var/log/sharelatex/web.log web.log` and additionally, copies of any of the following logs relevant to the issue:

- A copy of the **clsi.log** to help with project compiling issues. You can get a copy of this log from the container by running this command: 
`docker cp sharelatex:/var/log/sharelatex/clsi.log clsi.log`
- A copy of the **git-bridge.log** to help with the Git Bridge integration. You can get a copy of this log from the container by running this command: 
`docker logs git-bridge > git-bridge.log`
- A copy of the **project-history.log** to help with project history issues. You can get a copy of this log from the container by running this command: 
`docker cp sharelatex:/var/log/sharelatex/project-history.log project-history.log`
- A copy of the **history-v1.log** to help with project history issues. You can get a copy of this log from the container by running this command: 
`docker cp sharelatex:/var/log/sharelatex/history-v1.log history-v1.log`
- A copy of the **MongoDB** logs to help with database related issues. You can get a copy of this log from the container by running this command: 
`docker logs mongo > mongodb.log`

Once we have this information, we'll be able to look into this issue for you and provide you with some next steps.

### Live troubleshooting sessions ###

Unfortunately, while there are benefits associated with live troubleshooting sessions, due to the nature of the application being an on-premise solution there are a number of reasons we're unable to provide them, the most important being privacy/security, ensuring that no sensitive information is inadvertently shared (this is why we ask for redacted configuration files). 

Live troubleshooting is not always the most effective method when we encounter an issue for the first time. In these cases, issues require thorough investigation and testing, which shouldn't be rushed in a live session (even if this is in a test environment) as steps can be overlooked and resolutions not properly documented. 

Instead, we focus on asynchronous support, allowing us to provide secure, considered troubleshooting steps as quickly as possible, with most support queries responded to within one business day.