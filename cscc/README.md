![CSCC logo](https://www.cscc.edu/employee/communications/marketing/images/CSLogoWhiteonBlue.jpg)

- [Issues with CSCC VMs and docker compose](#issues-with-cscc-vms-and-docker-compose)
  - [Confirmed working in CSCC VMs](#confirmed-working-in-cscc-vms)
  - [some things to try](#some-things-to-try)
  - [Try the compose example](#try-the-compose-example)
- [Getting compose to run in a container (fix for old CSCC VMs)](#getting-compose-to-run-in-a-container-fix-for-old-cscc-vms)


# Issues with CSCC VMs and docker compose

In the past, the CSCC VMs had an older version of docker compose running that limited our ability to use examples. I believe that is fixed now and the CSCC VM image only behind a few minor versions.

```
# CSCC VM
$ docker compose version
Docker Compose version v2.21.0

# laptop
$  docker compose version
Docker Compose version v2.17.3

```

## Confirmed working in CSCC VMs

- [Wordpress - MySQL](../wordpress-mysql/)
- [Flash-Redis](../flask-redis/) 

## some things to try

You will need to open two terminals. I could not figure out a way to stop a container without opening a second termina and running a "docker stop."

- docker stop <container name>
- docker rm -f <container name>
- docker logs <container name>
- docker exec -it <container name>
- docker ps

## Try the compose example

Try the compose example based on the [docker documentation example.](./compose-example/)


# Getting compose to run in a container (fix for old CSCC VMs)

To run these examples, we need a newer version of docker compose. Lets run that in a container.

Here is what we are doing:

```bash

docker run --rm \
   --volume /var/run/docker.sock:/var/run/docker.sock \ #bind mounts a volume. This mounts the docker.sock on your VM to the docker.sock in the container
   --volume "$PWD:$PWD" \ # this bind mounts the current path, to the working directory in docker compose container
   --workdir="$PWD" \ #sets the working directory to the default path in the container
   docker-compose:latest \ # the image we are running. the container we pulled down with docker pull
   up
```

```bash
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v "$PWD:$PWD" \
  -w="$PWD" \
  lscr.io/linuxserver/docker-compose:latest \
  up
```