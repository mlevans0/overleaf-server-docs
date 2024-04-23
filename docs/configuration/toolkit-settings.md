<style>
table th:first-of-type {
    width: 40%;
    word-break: break-all;
}
table th:nth-of-type(2) {
    width: 60%;
}
</style>

This document describes the variables that are supported in the `config/overleaf.rc` file for {{ versions['toolkit-short'] }} deployments.

The `config/overleaf.rc` file consists of variable definitions in the form `NAME=value` , lines beginning with `#` are treated as comments.

!!! note

    We recommend that you re-create the Docker containers after changing anything in `overleaf.rc` or `variables.env` by running `bin/up`.

## Container ##

### `sharelatex` ###

| Name | Description |
|------|-------------|
| `PROJECT_NAME` | Sets the value of the `--project-name` flag supplied to `docker-compose`. This is useful when running multiple instances of Overleaf on one host, as each instance can have a different project name.<br /><br />- **Default**: `overleaf` |
| `SERVER_PRO` | When set to `true`, tells the {{ versions['toolkit-short'] }} to use the {{ versions['server-pro-short'] }} image (`quay.io/sharelatex/sharelatex-pro`), rather than the default {{ versions['community-edition-short'] }} image (`sharelatex/sharelatex`).<br />- **Default**: `false` |
| `SIBLING_CONTAINERS_ENABLED` | When set to `true`, tells the {{ versions['toolkit-short'] }} to use the **Sibling Containers** technique for compiling projects in separate sandboxes, using a separate Docker container for each project. See the [Sandboxed Compiles](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles) documentation for more information.<br /><br />- **Requires**: `SERVER_PRO=true`<br />- **Default**: `false` |
| `DOCKER_SOCKET_PATH` | Sets the path to the Docker socket on the host machine (the machine running the {{ versions['toolkit-short'] }}). When `SIBLING_CONTAINERS_ENABLED` is `true`, the socket will be mounted into the container, to allow the compiler service to spawn new Docker containers on the host.<br /><br /> - **Requires**: `SIBLING_CONTAINERS_ENABLED=true`<br />- **Default**: `/var/run/docker.sock` |
| `OVERLEAF_DATA_PATH` | Sets the path to the directory that will be mounted into the main `sharelatex` container, and used to store compile data. This can be either a full path (beginning with a `/`), or relative to the base directory of the {{ versions['toolkit-short'] }}.<br /><br />- **Default**: `data/sharelatex` |
| `OVERLEAF_LISTEN_IP` | Sets the host IP address(es) that the container will bind to. For example, if this is set to `0.0.0.0`, then the web interface will be available on any host IP address. For direct container access the value of `OVERLEAF_LISTEN_IP` must be set to your public IP address. Setting `OVERLEAF_LISTEN_IP` to either `0.0.0.0` or the external IP of your host will typically cause errors when used in conjunction with the [TLS Proxy](tls-proxy.md).<br /><br />- **Default**: `127.0.0.1` |
| `OVERLEAF_PORT` | Sets the host port that the container will bind to. For example, if this is set to `8099` and `OVERLEAF_LISTEN_IP` is set to `127.0.0.1`, then the web interface will be available on `http://localhost:8099`.<br /><br />- **Default**: `80` |

### `mongo` ###

| Name | Description |
|------|-------------|
| `MONGO_ENABLED` | When set to `true`, tells the {{ versions['toolkit-short'] }} to create a MongoDB container to host the database. When set to `false`, this container will not be created, and the system will use the MongoDB database specified by `MONGO_URL` instead.<br /><br />- **Default**: `true` |
| `MONGO_URL` | Specifies the MongoDB connection URL to use when `MONGO_ENABLED` is `false`<br /><br />- **Default**: not set |
| `MONGO_DATA_PATH` | Sets the path to the directory that will be mounted into the `mongo` container, and used to store the MongoDB database. This can be either a full path (beginning with a `/`), or relative to the base directory of the toolkit. This option only affects the local `mongo` container that is created when `MONGO_ENABLED` is `true`.<br /><br />- **Default**: `data/mongo` |

### `redis` ###

| Name | Description |
|------|-------------|
| `REDIS_ENABLED` | When set to `true`, tells the {{ versions['toolkit-short'] }} to create a Redis container, to host the redis database. When set to `false`, this container will not be created, and the system will use the Redis database specified by `REDIS_HOST` and `REDIS_PORT` instead.<br /><br />- **Default**: `true` |
| `REDIS_HOST` | Specifies the Redis host to use when `REDIS_ENABLED` is `false`<br /><br />- **Default**: not set |
| `REDIS_PORT` | Specifies the Redis port to use when `REDIS_ENABLED` is `false`<br /><br />- **Default**: not set |
| `REDIS_DATA_PATH` | Sets the path to the directory that will be mounted into the `redis` container, and used to store the Redis database. This can be either a full path beginning with a `/`), or relative to the base directory of the {{ versions['toolkit-short'] }}. This option only affects the local `redis` container that is created when `REDIS_ENABLED` is `true`.<br /><br />- **Default**: `data/redis` |

### `nginx` ###

| Name | Description |
|------|-------------|
| `NGINX_ENABLED` | When set to `true`, tells the {{ versions['toolkit-short'] }} to create an NGINX container, to act as a [TLS Proxy](tls-proxy.md).<br /><br />- **Default**: `false` |
| `NGINX_CONFIG_PATH` | Path to the NGINX config file to use for the [TLS Proxy](tls-proxy.md).<br /><br />- **Default**: `config/nginx/nginx.conf` |
| `NGINX_TLS_LISTEN_IP` | Sets the host IP address(es) that the [TLS Proxy](tls-proxy.md) container will bind to for HTTPS. For example, if this is set to `0.0.0.0` then the HTTPS web interface will be available on any host IP address. Typically this should be set to the external IP of your host.<br /><br />- **Default**: `127.0.1.1`|
| `NGINX_HTTP_LISTEN_IP` | Sets the host IP address(es) that the [TLS Proxy](tls-proxy.md) container will bind to for http redirect. For example, if this is set to `127.0.1.1` then HTTP connections to `127.0.1.1` will be redirected to the HTTPS web interface. Typically this should be set to the external IP of your host. Do not set it to `0.0.0.0` as this will typically cause a conflict with `OVERLEAF_LISTEN_IP`. <br /><br />- **Default**: `127.0.1.1` |
| `NGINX_HTTP_PORT` | Sets the host port that the [TLS Proxy](tls-proxy.md) container will bind to for HTTP.<br /><br />- **Default**: `80` |
| `TLS_PORT` | Sets the host port that the [TLS Proxy](tls-proxy.md) container will bind to for HTTPS.<br /><br />- **Default**: `443` |
| `TLS_PRIVATE_KEY_PATH` | Path to the private key to use for the [TLS Proxy](tls-proxy.md).<br /><br />- **Default**: `config/nginx/certs/overleaf_key.pem` |
| `TLS_CERTIFICATE_PATH` | Path to the public certificate to use for the [TLS Proxy](tls-proxy.md).<br /><br />- **Default**: `config/nginx/certs/overleaf_certificate.pem` |