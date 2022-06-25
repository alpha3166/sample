# Usage

## Setup

Replace UID (`1000`) and GID (`1000`) in `Dockerfile` and `docker-compose.yml` with yours.

## With Docker

To build image:

    docker build -t template-dev .

To start shell:

    docker run -it --rm -u $(id -u):$(id -g) -v $PWD/../..:/template -w /template template-dev

## With Docker-Compose

To start shell:

    docker-compose run --rm ws
