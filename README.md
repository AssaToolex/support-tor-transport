
# TOR-transport

[![GitHubCI](https://github.com/-/-.svg?style=svg)](https://github.com/-/-/talex/support-tor-transport)

(c) 2014-2020 Assa Toolex

Transport on the basis of TOR for the support
and access

Redistribution and modifications are under
the terms of GPLv3 license.

[Tor][1] was to have a way to use the internet
with as much privacy as possible, and the idea
was to route traffic through multiple servers
and encrypt it each step of the way.

This docker package **Tor-transport**
under [Alpine Linux][2].

Visit [Docker Hub][3] to see all available tags.

[1]: https://www.torproject.org/
[2]: https://alpinelinux.org/
[3]: https://hub.docker.com/r/talex/support-tor-transport/


## Run

Simple run this container is:
```sh
$ sudo docker run -d \
    --name=support-tor-transport \
    -v /srv/support-tor-transport/cfg:/etc/tor \
    -v /srv/support-tor-transport/data:/var/lib/tor \
    talex/support-tor-transport
```
This start `tor`-core with config
in `/srv/support-tor-transport/cfg` and data
in `/srv/support-tor-transport/data` persistent volumes.

If you want to add parameters for `tor`-core, 
you just need to add the line in `torrc` config file.
You must to create config file if it not exist.
And to set as data directory and set owner
of `/srv/support-tor-transport/data` to UID and GID 100.

```sh
$ sudo mkdir -p /srv/support-tor-transport/cfg \
    /srv/support-tor-transport/data
$ echo "DataDirectory /var/lib/tor" | sudo \
    tee --append /srv/support-tor-transport/cfg/torrc
$ sudo chown 100:100 /srv/support-tor-transport/data
```


## Samples

### SOCKS Proxy

To use as socks proxy just 
set SOCKSPort in config 
file `/srv/support-tor-transport/cfg/torrc`
```
SOCKSPort 0.0.0.0:1080
DataDirectory /var/lib/tor
```
and run:
```sh
$ sudo docker run -d \
    --name=support-tor-transport \
    -p 127.0.0.1:1080:1080 \
    -v /srv/support-tor-transport/cfg:/etc/tor \
    -v /srv/support-tor-transport/data:/var/lib/tor \
    talex/support-tor-transport
```
Set `127.0.0.1:1080` as socks proxy in your browser,
and go to https://check.torproject.org/ to check
if you are using the Tor network (or any network).


### Hidden Tor-Services

To use as hidden tor-services server just 
set parameters in config 
file `/srv/support-tor-transport/cfg/torrc`
```
SOCKSPort 0
DataDirectory /var/lib/tor
HiddenServiceDir /var/lib/tor/mail.com/
HiddenServicePort 80  82.165.229.87:80
HiddenServicePort 443 82.165.229.87:443
```
and run:
```sh
$ sudo docker run -d \
    --name=support-tor-transport \
    -v /srv/support-tor-transport/cfg:/etc/tor \
    -v /srv/support-tor-transport/data:/var/lib/tor \
    talex/support-tor-transport
```


## Manual builds

We use sudo !!!
```sh
$ git ....
$ cd components/transport/support-tor
$ make docker-build
...
Successfully built 043d94ecce48
Successfully tagged talex/support-tor-transport:latest
```


## Manual push to dockerhub

We use sudo !!!
```sh
$ git ....
$ cd components/transport/support-tor
$ make docker-build
$ DOCKERHUB_USERNAME=user DOCKERHUB_PASSWORD=pwd CIRCLE_TAG=new make dockerhub-push
```
