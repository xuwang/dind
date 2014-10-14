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
git clone https://github.com/xuwang/docker-echo
docker run --privileged -v ./docker-echo:/var/cache/docker-echo dind dkbuild xuwang/docker-echo /var/cache/docker-echo
```
## Usage with [Drone.io](drone.io)

_.drone.yml_

```
image: xuwang/dind
script:
  - dkbuild xuwang/docker-echo  /var/cache/drone/src/github.com/xuwang/docker-echo
```


