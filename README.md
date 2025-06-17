
# Minimal Tor client container

This container provides internet access by connecting to the Tor network.

It is designed to be used as a Tor client for other containers or the host system, 
allowing them to access the Tor network without exposing any ports.

## Features

- not exposes any ports
- small image size
- uses minimal tor configuration
- usable for containers and host system

## Example usage

### For host

Run tor client container in detached mode:
```bash
docker run -d --name tor-client \
  --restart unless-stopped \
  -p 9050:9050 \
  smartondev/tor-client:latest
```

Access Tor network from the host:
```bash
curl --socks5-hostname localhost:9050 https://ident.me
```

### For containers

Run tor client container in detached mode:
```bash
docker run -d --name tor-client \
  --restart unless-stopped \
  smartondev/tor-client:latest
```

Create a tor network without external access:
```bash
docker network create --internal tor-net
```

Attach tor client to the network:
```bash
docker network connect tor-net tor-client
```

Attach any container to the tor network
```bash
docker run --network tor-net \
  ...
```

And access Tor network from the container:
```bash
curl --socks5-hostname tor-client:9050 https://ident.me
```

Topology of example usage:

```
                      +----------------------------+
                      |                            |
Internet <-> Tor <--> |   smartondev/tor-client    |
                      |                            |
                      +----+-----------------------+
                           |
                           | tor-net (internal network)
                           |
           +---------------+---------------+
           |               |               |
+----------v-----+ +-------v------+ +------v-------+
|                | |              | |              |
| container-1    | | container-2  | | container-3  |
| (e.g. curl)    | | (e.g. wget)  | | (e.g. app)   |
|                | |              | |              |
+----------------+ +--------------+ +--------------+
```

## Source code

The source code of this container is available on [GitHub](https://github.com/smartondev/tor-client).

## Docker images versions

- `nightly`: `smartondev/tor-client:nightly` (latest build)
- `x.y.z`: `smartondev/tor-client:x.y.z` (stable or development version)
- `latest`: `smartondev/tor-client:latest` (latest stable or development version)

## Author

[Márton Somogyi](https://github.com/kamarton)
