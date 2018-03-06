# Starbelly: Docker

This repository contains Dockerfiles and Docker Compose files for [the Starbelly
crawler](https://gitlab.com/hyperion-gray/starbelly). This README explains how
to build Docker images. For information on running Docker containers, see the
[Starbelly documentation](https://starbelly.readthedocs.io).

## Production

The `starbelly` directory contains Dockerfiles to build two production images
(`app` and `web`) as well as a Docker Compose file to run the production
application. In practice, production images are published to Docker Hub, so you
shouldn't need to build your own images.

You need some dependencies to build Docker images. This assumes that you have
the `starbelly` and `starbelly-web-client` repos checked out in the same
directory as `starbelly-docker`. `$STARBELLY_ROOT` must point to the path that
contains all of these repos. Perform this intial setup:

    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly/app/dependencies/starbelly
    $ git clone ../../starbelly
    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly/web/dependencies/starbelly-web-client
    $ git clone ../../starbelly-web-client

When you want to update the Docker images to match your local repos, you can run
`git pull` in each of these repos, but when you want to produce official release
images, you should `git clean -fx && git checkout $STARBELLY_TAG`, where the tag
variable contains the version tag that is being released, e.g. `1.0.0`.

Next, then run these commands:

    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly/app
    $ docker build -t hyperiongray/starbelly-app:$STARBELLY_TAG .
    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly/web
    $ docker build -t hyperiongray/starbelly-web:$STARBELLY_TAG .

To upload release images to Docker Hub:

    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly/app
    $ docker push hyperiongray/starbelly-app:$STARBELLY_TAG
    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly/web
    $ docker push hyperiongray/starbelly-web:$STARBELLY_TAG

## Development

The `starbelly-dev` directory contains Dockerfiles and Docker Compose file to
run a development instance of Starbelly. A development instance mounts source
code from the host file system into the container's file system, so that you can
edit code on the host and run/debug that code inside the container.

The development setup requires you to place the `starbelly` and `starbelly-web-
client` repos in the same directory as this `starbelly-docker` repo (same layout
as the production section above). It also expects you to place your host's pub
cache at `/var/cache/pub`. To set this up:

    $ sudo mkdir /var/cache/pub
    $ sudo chown $USER:$USER /var/cache/pub

Now, export `PUB_CACHE=/var/cache/pub` in your environment (probably in your
`.profile` or `.bash_aliases`).

You should run `pub get` and `pub upgrade` from your host, not from your
container, so that pub will populate packages into the host's pub cache with the
correct permissions. This setup has the benefit of making pub packages available
for your IDE to analyze and to enable auto-complete.

To build development images:

    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly-dev/app
    $ docker build -t starbelly-dev-app .
    $ cd $STARBELLY_ROOT/starbelly-docker/starbelly-dev/web
    $ docker build -t starbelly-dev-web .

We recommend that you *do not* use a version tag when building the dev images,
and you definitely shouldn't push developer images to Docker Hub.
