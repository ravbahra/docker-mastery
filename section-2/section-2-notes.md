# Section 2

## Verifying installation
```{bash}
docker version
```
returns both client and server version, server is the engine and client is the cli, us it to ensure the cli can talk to the engine

`docker info` gives information such as the number of cpus being used etc, total mem
```{bash}
docker info
```

new command line structure
```{bash}
docker <management-command> <sub-command>
docker container run
```
although `docker run` still works, but they organised stuff into management commands and sub commands

## Installing NginX web server

```{bash}
docker container run --publish 80:80 nginx
```

Then navigate to http://localhost to check it works

this command pulled the image called nginx, the publish routes traffic from port 80 to the local machine to port 80 of the container, format is `<local ip>:<container ip>` 

`ctrl+c` to terminate your container

## Starting a container in background mode 

use the `--detach` flag as below

```{bash}
docker container run --publish 80:80 --detach nginx
```

```{bash}
docker container ls
```

## Stopping a running container in background mode

```{}
docker container stop <id>
```

to get the ID run `docker container ls`, you only need to enter enough of the ID to be unique, do you don't need to enter the entire ID

## specifying names
names are generated from a list of adjectives and hacker names, you can see the name from `docker container ls`, however you can specify a name with `--name`


```{bash}
docker container run --publish 80:80 --detach --name test01 nginx

# i've also seen detached mode like
# --detach or -d
docker container run -d
```

```{bash}
# will show all containers, stopped and running
docker container ls -a 

# will show just running containers
docker container ls

# remove all the containers (can't remove running containers)
docker container rm <CONTAINER_ID0> <CONTAINER_ID1> ...

# stop running containers by using stop command or force rm
docker container stop <container_id>
docker container rm -f <container_id>

docker container start <container_name>

# show logs for container
docker container logs <container_name>

# show running processes in the container
docker container top <container_name>

# run in interactive mode (you can interact and launch a command)
docker container run -it microsoft/nanoserver powershell

# setting an environment variable using -e or --env
docker container run -d -p 3606:3606 mysql --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

```

# Summary of 3 modes

## run the process and exit when finished

`docker container run`

## run in interactive mode (you get a shell)

`docker container run -it`

the -t = tty
-i = keep it open
- [-t] --tty = terminal
- [-i] --interactive 

## run in detached mode (background process)

`docker container run -d`

# What's going on in Containers

```
# process list in one container
docker container top 

# details of one container config
docker container inspect

# performance stats for all containers (cpu usage, mem etc)
docker container stats

```

## passing a command when the container starts up

some containers might actually have some commands that start when the container starts, but you can pass a command when it starts. For example if we want the bash prompt to start when the container starts we could do

`docker container run -it --name proxy nginx bash`

## Alpine linux

`docker contain run -t alpine bash`

this will give an error, bash doesn't come in the alpine image but it does come with `sh`

`docker contain run -t alpine sh`
alpine package manager is apk

there is an nginx distribution running on alpine, this also has the ping command which was removed from the 

`docker container run nginx:alpine`

# 19. Docker Networking

`docker container run -p` exposes the port on your machine

`docker contaianer port <container>` tells you what ports are open 

- Containers connected to a private virtual network bridge
- these virtual network routes through a NAT firewall on the host IP
- don't need to use -p for containers to communicate, create a separate network for related containers

- skip virtual networks and use host IP `--net=host`

`docker container run -p 80:80 --name webhost -d nginx`

## find out port mappings on a container

- `docker container port webhost`
- `80/tcp -> 0.0.0.0:80` <- result

## find the IP of the container

`docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <container_name>`

## manging networks with cli

- `docker network ls`
- `docker network inspect`
- `docker network create --driver`
- `docker network connect <network> <container>` connect a live container

## creating a network

by default a new network gets the driver bridge, but you specify a driver eg for overlay networks (networks between hosts)
`docker network create my_app_net`

then to create a container attached to that network run

`docker container run -d --name nginx2 --network my_app_net nginx`

`docker network inspect my_app_net`