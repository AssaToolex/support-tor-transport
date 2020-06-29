
# Support TOR-transport

[![DockerHUB Image CI](https://github.com/AssaToolex/support-tor-transport/workflows/DockerHUB%20Image%20CI/badge.svg)](https://hub.docker.com/r/talex/support-tor-transport)

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

Visit [GitHub][4] to see a source code.

[1]: https://www.torproject.org/
[2]: https://alpinelinux.org/
[3]: https://hub.docker.com/r/talex/support-tor-transport/
[4]: https://github.com/AssaToolex/support-tor-transport/


## Run

We are use sudo !!!
Simple run this container is:
```sh
$ sudo docker run -d talex/support-tor-transport:stage-alpine-latest
```
This start `tor`-core with default empty config
in the container directory `/etc/tor/`.


## Samples

### Transport configuration as a SOCKS-proxy

To use as a socks proxy just set SOCKSPort in 
config file.
If you want to add parameters for `tor`-core, then 
you just need to add the line in `/etc/tor/torrc` 
config file in persistent volume and mount it.
```sh
$ sudo mkdir -p /srv/support-tor-transport
$ echo "SOCKSPort 0.0.0.0:1080" | sudo \
    tee --append /srv/support-tor-transport/torrc
```
please, use 0.0.0.0 as your ip and any of the following 
443, 80, 8080, 1080, 1081, 9050, 9051, 9150, 9151 as 
a port and run:
```sh
$ sudo docker run -d \
    --name=support-tor-transport-1081 \
    -p 127.0.0.1:1081:1080 \
    -v /srv/support-tor-transport:/etc/tor \
    talex/support-tor-transport:stage-alpine-latest
```
Set `127.0.0.1:1081` as socks proxy in your browser,
and go to https://check.torproject.org/ to check
if you are using the Tor network (or any network).
```sh
$ curl --socks5-hostname 127.0.0.1:1081 \
    https://check.torproject.org/
$ curl --socks5-hostname 127.0.0.1:1081 \
    https://ifconfig.me
```

### Transport data exchange using Hidden Tor-Services as an example

To use as hidden tor-services server just 
set parameters in config file
```
HiddenServiceDir /etc/tor/mail.com/
HiddenServicePort 80  82.165.229.87:80
HiddenServicePort 443 82.165.229.87:443
```
If you want to add or retrieve parameters for/from 
`tor`-core, then you just need mounted persistent 
volume and add the parameter line in `/etc/tor/torrc` 
config file. You must to create files/folders in 
persistent volume and set owner to UID and GID 100.
```sh
$ sudo mkdir -p /srv/support-tor-transport \
    /srv/support-tor-transport/mail.com
$ sudo chown 100:100 \
    /srv/support-tor-transport/mail.com
$ sudo chmod 700 \
    /srv/support-tor-transport/mail.com
$ sudo tee /srv/support-tor-transport/torrc << EOF
SOCKSPort 0
HiddenServiceDir /etc/tor/mail.com/
HiddenServicePort 80  82.165.229.87:80
HiddenServicePort 443 82.165.229.87:443
Log notice stdout
EOF
$
```
and run:
```sh
$ sudo docker run -d \
    --name=support-tor-transport-mc \
    -v /srv/support-tor-transport:/etc/tor \
    talex/support-tor-transport:stage-alpine-latest
```
Get `onion`-name:
```sh
$ sudo cat /srv/support-tor-transport/mail.com/hostname
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.onion
```
Check, how it work:
```sh
$ curl --socks5-hostname 127.0.0.1:1081 \
    https://`sudo cat /srv/support-tor-transport/mail.com/hostname`
```
