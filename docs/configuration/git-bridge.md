The Git integration is available since {{ versions['server-pro-short'] }} 4.0.1

User documentation for this feature can be found [here](https://www.overleaf.com/learn/how-to/Using_Git_and_GitHub#The_Overleaf_Git-Bridge).

If you’re using the [{{ versions['toolkit-full'] }}](https://github.com/overleaf/toolkit), the git-bridge can be enabled by setting `GIT_BRIDGE_ENABLED=true` in your `config/overleaf.rc` file.

For users running a custom `docker-compose.yml`, add the following container configuration to your compose file:

```yml
git-bridge:
    restart: always
    image: quay.io/sharelatex/git-bridge:4.0.0 # tag should match the `sharelatex` container tag
    volumes:
        - ~/git_bridge_data:/data/git-bridge
    container_name: git-bridge
    expose:
        - "8000"
    environment:
        GIT_BRIDGE_API_BASE_URL: "http://sharelatex:3000/api/v0/" # "http://sharelatex/api/v0/" for version 4.1.6 and earlier
        GIT_BRIDGE_OAUTH2_SERVER: "http://sharelatex"
        GIT_BRIDGE_POSTBACK_BASE_URL: "http://git-bridge:8000"
        GIT_BRIDGE_ROOT_DIR: "/data/git-bridge"
    user: root
    command: ["/server-pro-start.sh"]
```

You’ll also need to add a link to the `git-bridge` container in the `sharelatex` container, and define new environment variables:   

```yml
sharelatex:
    links:
        - git-bridge
    environment:         
         GIT_BRIDGE_ENABLED: true
         GIT_BRIDGE_HOST: "git-bridge"
         GIT_BRIDGE_PORT: "8000"
         V1_HISTORY_URL: "http://sharelatex:3100/api"
```
When authenticating a git client, users need a Personal Access Token (note that overleaf.com supports username/password, this is not the case for {{ versions['server-pro-short'] }}). Users can manage the Personal Access Tokens through the application UI (see the [documentation](https://www.overleaf.com/learn/how-to/Git_integration_authentication_tokens?preview=true)).

We recommend you monitor your host resources after enabling the git-bridge. The load increase will depend on the number of users accessing the feature and the type of projects hosted in your instance (larger projects will generally be more resource intensive). 

### Swapping projects to S3

The Git integration stores a complete git repository on disk for each project that gets cloned by a user. If you have limited disk space, you can activate a swap job that will move repositories that are less used to AWS S3. If a swapped repository is needed again, it gets moved back to the disk. The following environment variables control the swap job:

- `GIT_BRIDGE_SWAPSTORE_TYPE`: set this to “s3” to activate the swap job
- `GIT_BRIDGE_SWAPSTORE_AWS_ACCESS_KEY`: your AWS access key
- `GIT_BRIDGE_SWAPSTORE_AWS_SECRET`: your AWS secret
- `GIT_BRIDGE_SWAPSTORE_S3_BUCKET_NAME`: this bucket will contain the zipped git repositories
- `GIT_BRIDGE_SWAPSTORE_AWS_REGION`: the bucket’s region
- `GIT_BRIDGE_SWAPJOB_MIN_PROJECTS`: how many projects to keep on disk, at a minimum. (default: 50)
- `GIT_BRIDGE_SWAPJOB_LOW_GIB`: low watermark for swapping. The swap job will move projects until disk usage is below this value. (default: 128 GB)
- `GIT_BRIDGE_SWAPJOB_HIGH_GIB`: high watermark for swapping. The swap job will start swapping when disk usage reaches this value. (default: 256 GB)
- `GIT_BRIDGE_SWAPJOB_INTERVAL_MILLIS`: amount of time between checking disk usage and running the swap job. (default: 3600000 ms = 1 hour)