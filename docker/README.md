# Galene in Docker

Build the image from the top of the source tree:

```sh
docker build -f docker/Dockerfile -t galene .
```

The volume `galene` holds `data`, `groups` and `recordings`.  Arguments
after the image name are passed to Galene.  Pick the case that matches
your setup.

## Linux server with a public IP address

```sh
docker run -d --name galene --network host -v galene:/galene galene
```

If the server has a firewall, see *Running behind a firewall* in
`galene-install.md`.

## Linux host behind NAT

For example an office server or a NAS behind a router.  Forward TCP 8443
and TCP and UDP 1194 on the router, and give Galene the public address:

```sh
docker run -d --name galene --network host -v galene:/galene \
    galene -turn 203.0.113.1:1194
```

If all users are on the same LAN, `-turn` and port forwarding are not
needed.

## Windows or macOS (Docker Desktop)

Host networking does not give Galene the host's addresses here, so
publish the ports and set `-turn` to the address at which users reach the
host, the LAN address for an office, or the public address:

```sh
docker run -d --name galene -v galene:/galene \
    -p 8443:8443 -p 1194:1194 -p 1194:1194/udp \
    galene -turn 192.168.1.10:1194
```

All media then goes through Galene's TURN server.

## Behind a reverse proxy

Run Galene as for a public server, with `-insecure`, and follow *Running
behind a reverse proxy* in `galene-install.md`.

## Groups

Set up `galenectl` and create a group:

```sh
docker exec -it galene galenectl -admin-username admin \
    initial-setup -config data/config.json
docker exec galene galenectl -insecure create-group -group city-watch
```
