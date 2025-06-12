
# Minimal tor client container

This container provides internet access by connecting to the Tor network.

## Features

- not exposes any ports
- small image size
- uses minimal tor configuration

## Example usage

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
curl --socks5-hostname tor-client https://ident.me
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

## Docker images versions

- `nightly`: `smartondev/tor-client:nightly` (latest build)
- `x.y.z`: `smartondev/tor-client:x.y.z` (stable or development version)
- `latest`: `smartondev/tor-client:latest` (latest stable or development version)

## Author

[Márton Somogyi](https://github.com/kamarton)
