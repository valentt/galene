# Galene in Docker

Build the image from the top of the source tree:

```sh
docker build -f docker/Dockerfile -t galene .
```

Galene is designed to be exposed directly to the Internet, so run it with
host networking:

```sh
docker run -d --name galene --network host \
    -v galene-data:/galene/data \
    -v galene-groups:/galene/groups \
    -v galene-recordings:/galene/recordings \
    galene
```

Any arguments after the image name are passed to Galene.  If the host is
behind NAT, end the command with `galene -turn 203.0.113.1:1194`, using
the public address, and see *Advanced configuration* in
`galene-install.md`.

Docker Desktop on Windows and macOS runs containers in a virtual machine,
so host networking does not work there.  Publish the ports instead, and
give Galene the address at which clients reach the host:

```sh
docker run -d --name galene \
    -p 8443:8443 -p 1194:1194 -p 1194:1194/udp \
    -v galene-data:/galene/data \
    -v galene-groups:/galene/groups \
    -v galene-recordings:/galene/recordings \
    galene -turn 203.0.113.1:1194
```

Media then goes through Galene's TURN server.

Set up `galenectl` and create a group:

```sh
docker exec -it galene galenectl -admin-username admin \
    initial-setup -config data/config.json
docker exec galene galenectl -insecure create-group -group city-watch
```
