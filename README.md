# Docker Registry

This repository can be used to spin up a registry on a host running docker.
I will follow the [docker guide to deploying a registry](https://docs.docker.com/registry/deploying/) as much as possible.


## Prerequisites

- A provisioned host running docker
- Git (or a copy of this repository)
- docker-compose installed

## Usage

Make sure the registry version in the command below is the same as in your compose file to avoid two different versions of the registry image to be pulled. On the registry host run the following:

```
git clone git@github.com:dhensen/docker-registry.git
cd docker-registry
 export REGISTRY_USER=your_username_here
 export REGISTRY_PASS=your_password_here
docker run \
  --entrypoint htpasswd \
  registry:2.6.2 -Bbn $REGISTRY_USER $REGISTRY_PASS > auth/htpasswd
cp /path/to/fullchain1.pem certs/fullchain1.pem
cp /path/to/privkey1.pem certs/privkey1.pem
docker-compose up -d
```

On your workstation run the following:

```
docker login registry.yourdomain.tld:5000
```

You will be prompted for the username and password you provided while setting up on the host.
You can also login like this:

```
 export REGISTRY_USER=your_username_here
 export REGISTRY_PASS=your_password_here
docker login -u $REGISTRY_USER -p $REGISTRY_PASS registry.yourdomain.tld:5000
```

You will get a warning that using `--password` via the CLI is insecure. The insecurity comes from the fact that someone can read
the password in plain text from your shell history. You can mitigate using env vars, but then make sure to type a space before the `export` commands to skip your shell from saving these commands. (Most shells have this behaviour, but please double check this for your shell)

If you want to be totally safe, create a file that only your user can read with the password in it, then execute:

```
 export REGISTRY_USER=your_username_here
vim ./the_secret_containing_file
cat ./the_secret_containing_file | docker login -u $REGISTRY_USER --password-stdin registry.yourdomain.tld:5000
```