


## Manual builds

We are use sudo and make.
```sh
$ git ....
$ cd components/transport/support-tor
$ make docker-build
...
Successfully built 043d94e41e48
Successfully tagged talex/support-tor-transport:latest
```


## Manual push to dockerhub

We are use sudo and make.
```sh
$ git ....
$ cd components/transport/support-tor
$ make docker-build
$ DOCKERHUB_USERNAME=user DOCKERHUB_PASSWORD=pwd CIRCLE_TAG=new make dockerhub-push
```
