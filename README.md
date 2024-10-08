# [Cloudflared-web](https://github.com/WisdomSky/Cloudflared-web)

_Cloudflared-web is a docker image that packages both cloudflared cli and a simple Web UI to easily start or stop remotely-managed Cloudflare tunnel._


[![build](https://github.com/WisdomSky/Cloudflared-web/workflows/Build/badge.svg)](https://github.com/WisdomSky/Cloudflared-web/actions "Build Status")
[![latest](https://img.shields.io/docker/v/wisdomsky/cloudflared-web/latest?label=Latest)](https://hub.docker.com/r/wisdomsky/cloudflared-web/tags "Latest Tag")
[![pulls](https://img.shields.io/docker/pulls/wisdomsky/cloudflared-web?label=Pulls)](https://hub.docker.com/r/wisdomsky/cloudflared-web "Docker Hub Pulls")
[![stars](https://img.shields.io/docker/stars/wisdomsky/cloudflared-web?label=%E2%AD%90)](https://hub.docker.com/r/wisdomsky/cloudflared-web "Docker Hub Stars")

---

## Why use `Cloudflared-web`?

#### Pros

✅ Only need to run a docker command once. No need to run docker commands everytime you want to start or stop the container or when updating the token.

✅ Start and stop cloudflare tunnel anytime with a single click.

#### Cons

❌ Only supports [Remotely-managed Tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/configure-tunnels/remote-management/).

❌ Can only update hostname policies through the [ZeroTrust](https://one.dash.cloudflare.com/) dashboard.


--- 
## Application Setup
When manually setting up this image, it is crucial to always set the `networking mode` into `host` as without it, the cloudflared service won't be able to access the services running on the host:

    docker run --network host wisdomsky/cloudflared-web:latest

or if using `docker-compose.yml`:

```yaml
services:
  cloudflared:
    image: wisdomsky/cloudflared-web:latest
    restart: unless-stopped
    network_mode: host
```

The Web UI where you can setup the Cloudflared token can be accessed from port `14333`:

    http://localhost:14333

### Github Containers

If for some reason you are unable to pull images from Docker's Official Image Registry (docker.io), `Cloudflared-web` is also synced to Github Container Registry (ghcr.io).

Just prefix the image with `ghcr.io/` in order to use the mirrored image in Github.
```yaml
services:
  cloudflared:
    image: ghcr.io/wisdomsky/cloudflared-web:latest
    restart: unless-stopped
    network_mode: host
```


---
## Additional Parameters

### Environment
| Variable Name   | Default value | Required or Optional | Description                                                                                                                                                                          |
|-----------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WEBUI_PORT      | 14333         | _Optional_ | The port on the host where the WebUI will be running. Useful when an existing process is running on port `14333` and want to assign cloudflared-web into a different available port. |
| PROTOCOL        | auto          | _Optional_ | Specifies the protocol used to establish a connection between cloudflared and the Cloudflare global network. Available values are `auto`, `http2`, and `quic`.                                              |
| METRICS_ENABLE  | false         | _Optional_ | Enable [tunnel metrics](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/monitor-tunnels/metrics/) server.                                              |
| METRICS_PORT    | 60123         | _Optional_ | Specify port to run tunnel metrics on. `METRICS_ENABLE` must be set to `true`.                                                                                                       |
| BASIC_AUTH_PASS |               | _Optional_ | Enable Basic Auth by specifying a password. If `BASIC_AUTH_USER` is not specified, the default value for username `admin` will be used.                                              |
| BASIC_AUTH_USER | admin         | _Optional_ | Specify the username for the Basic Auth.                                                                                                                                             |

example `docker-compose.yaml`:
```yaml
services:
  cloudflared:
    image: wisdomsky/cloudflared-web:latest
    restart: unless-stopped
    network_mode: host
    environment:
      WEBUI_PORT: 1111
```


### Volume
| Container Path | Required or Optional | Description |
|---|---|---|
| /config | _Optional_ | The path to the directory where the `config.json` file containing the Cloudflare token and start status will be saved.  |

example `docker-compose.yaml`:
```yaml
services:
  cloudflared:
    image: wisdomsky/cloudflared-web:latest
    restart: unless-stopped
    network_mode: host
    volumes:
      - /mnt/storage/cloudflared/config:/config
```

## Using Networks

You can use docker `networks` for a more fine-grained control of which containers/services your cloudflared-web container has access to.

```yaml
services:
  cloudflared:
    image: wisdomsky/cloudflared-web:latest
    restart: unless-stopped
    networks:
      - mynetwork
    environment:
      WEBUI_PORT: 1111
```

## Screenshots

![Screenshot 1](https://raw.githubusercontent.com/WisdomSky/Cloudflared-web/main/screenshot-1.png)

![Screenshot 2](https://raw.githubusercontent.com/WisdomSky/Cloudflared-web/main/screenshot-2.png)

---

## Issues

For any problems experienced while using the docker image, please [create a new issue](https://github.com/WisdomSky/Cloudflared-web/issues).
