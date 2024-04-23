The most significant change in this update is the inclusion of a new [real-time](https://github.com/sharelatex/real-time-sharelatex) service which handles the editor websocket connections. These were previously handled by the web service. If you are upgrading from a previous version of ShareLaTeX, there are some things you may need to update to get it all working:

### Installing the real-time service

First make sure you actually have the real-time service installed:

```
$ grunt install:real-time
```

### websocketsUrl

You should add a new line to your config file to include the new `websocketsUrl` parameter:

```
# settings.coffee
modules.exports =
    ...
    siteUrl: "http://sharelatex.example.com"
    websocketsUrl: "http://sharelatex.example.com"
    ...
```

This should be the same as your `siteUrl`.

### Reverse proxy settings

In development the editor connects to the real-time service at http://localhost:3026, a separate end point from the web service, hence the need for a configurable parameter. In production you likely have a reverse proxy set up, and need to forward any requests to /socket.io onto the real-time service rather than the web service.

See the [[Nginx as a Reverse Proxy]] page for an Nginx example, particularly the `location /socket.io` block.