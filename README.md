# Docker Registry

This repository can be used to spin up a registry on a host running docker.
I will follow the [docker guide to deploying a registry](https://docs.docker.com/registry/deploying/) as much as possible.


## Prerequisites

- A provisioned host running docker
- Git (or a copy of this repository)
- docker-compose installed

## Usage

Make sure the registry version in the command below is the same as in your compose file to avoid two different versions of the registry image to be pulled.

```
git clone git@github.com:dhensen/docker-registry.git
cd docker-registry
export REGISTRY_USER=your_username_here
export REGISTRY_PASS=your_username_here
docker run \
  --entrypoint htpasswd \
  registry:2.6.2 -Bbn $REGISTRY_USER $REGISTRY_PASS > auth/htpasswd
cp /path/to/fullchain1.pem certs/fullchain1.pem
cp /path/to/privkey1.pem certs/privkey1.pem
docker-compose up -d
```