---
tags:
  - Server Pro
---

If you want or need access to Overleaf documentation on your instance of {{ versions['server-pro-short'] }}, then set `OVERLEAF_PROXY_LEARN=true` (`SHARELATEX_PROXY_LEARN=true` for versions `4.x` and earlier) in `toolkit/variables.env` or add the environment variable to your `docker-compose.yml` file. Documentation will be enabled at the `/learn` path.

This will proxy requests to the main [Overleaf documentation site](https://www.overleaf.com/learn), where it is always up to date. 

!!! note 

    Your local server will require access to the internet so it can perform external `GET` requests to [overleaf.com](overleaf.com).