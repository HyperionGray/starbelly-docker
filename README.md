# Starbelly: Docker

This repository contains Dockerfiles and Docker Compose scripts for
[the Starbelly crawler](https://gitlab.com/hyperion-gray/starbelly).

## Production

The `starbelly` directory contains Dockerfiles to build two production images
(`app` and `web`) as well as a Docker Compose file to run the production
application. In practice, production images are published to Docker Hub, so
you shouldn't need to build your own images.

To build production images, first fetch the dependencies (TODO), then run
these commands:

    $ cd starbelly-docker/starbelly/app
    $ docker build -t hyperiongray/starbelly-app:1.0.0 .
    ...snip...
    $ cd ../web
    $ docker build -t hyperiongray/starbelly-web:1.0.0 .
    ...snip...

Substitute the actual release number instead of the `1.0.0` tag

## Development

The `starbelly-dev` directory contains Dockerfiles and Docker Compose file
to run a development instance of Starbelly. A development instance mounts source
code from the host file system into the container's file system, so that you can
edit code on the host and run/debug that code inside the container.

The development setup requires you to place the `starbelly` and
`starbelly-web-client` repos in the same directory as this `starbelly-docker`
repo. It also expects you to place your host's pub cache at `/var/cache/pub`.
repo. When you update the `pubspec.yaml`, you should run `pub get` from your
host, not in your container, so that pub will populate packages into the host's
pub cache. This setup has the benefit of making pub packages available for your
IDE to analyze and to enable auto-complete.

When running `starbelly-dev`, the `pub` daemon is not automatically started.
You should connect to the container, e.g.
`docker exec -it starbelly-dev-web /bin/bash`, and then run pub manually, e.g.
`cd /starbelly-web-client && pub serve --port 8001`.

To build development images:

    $ cd starbelly-docker/starbelly-dev/app
    $ docker build -t starbelly-dev-app .
    ...snip...
    $ cd ../web
    $ docker build -t starbelly-dev-web .
    ...snip...

We recommend that you *do not* use a version tag when building the dev images.
