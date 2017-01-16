# Starbelly: Docker

This repository contains Dockerfiles and Docker Compose scripts for
[the Starbelly crawler](https://gitlab.com/hyperion-gray/starbelly).

## Production

The `starbelly` directory contains Dockerfiles to build two production images
(`app` and `web`) as well as a Docker Compose file to run the production
application. In practice, production images are published to Docker Hub, so
you shouldn't need to build your own images.

## Development

The `starbelly-dev` directory contains Dockerfiles and Docker Compose file
to run a development instance of Starbelly. A development instance mounts source
code from the host file system into the container's file system, so that you can
edit code on the host and run/debug that code inside the container.
