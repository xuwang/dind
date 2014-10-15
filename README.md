#Docker-build-in-Docker

This recipe lets you build docker images within a docker.

It is based on [Docker-in-Docker](https://github.com/jpetazzo/dind) that did the heavy lifting.

This dind image add a docker build script, _dkbuild_ that builds docker iamge withing a docker.
This _dind_ and _dkbuild_ can be used in a CI system like Drone.io that build docker images.

_dkbuild_ is used within _dind_ and it takes two parameters:

```
dkbuild <repo/user/iamge> <Dockerfile dir>
```
it will build and push the image to the given repo.

_Note:_ There is only one requirement: your Docker version should support the
`--privileged` flag.

## Quickstart

Build the image:
```bash
docker build -t dind .
```

## Build a image in Docker

```
git clone https://github.com/xuwang/dind /tmp
docker run --rm --privileged \
    -v /tmp/dind:/var/cache/dind \
    xuwang/dind dkbuild registry.docker.local/xuwang/dind /var/cache/dind
```
## Usage with [Drone](https://github.com/drone/drone)

With Drone, before the native docker service is ready:
_.drone.yml_ file for build a docker image is fairly straightforward:
```
image: xuwang/dind
script:
  - dkbuild registry.docker.local/xuwang/docker-echo  /var/cache/drone/src/github.com/xuwang/docker-echo
```


