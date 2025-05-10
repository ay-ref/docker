# Docker

## Installation

- TODO

## docker without sudo

```sh
sudo groupadd docker 
sudo gpasswd -a $USER docker 
newgrp docker 
```

### centos

```shell
sudo yum remove docker docker-engine docker.io containerd runc
```

```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
```

```shell
systemctl start docker
systemctl status docker
systemctl stop docker
```

## CLI

- see the docker images

    ```shell
    docker images
    ```

- remove one image

    ```shell
    docker rmi <image-name>
    ```

- run a docker container

    ```shell
    docker run --name <container-name> -d <image-name>
    ```

    > flag `--rm` is used to remove the container after it stops.

- stop a docker container forcefully

    ```shell
    docker kill <container-id>
    ```

- see the running container

    ```shell
    docker ps
    ```

- see all running and stoped containers

    ```shell
    docker ps -a
    ```

- remove stoped docker containers

    ```shell
    docker container prune
    ```

- remove all docker containers (running and stoped) forcefully

    ```shell
    docker rm -vf $(docker ps -aq)
    ```

- remove all docker images:

    ```bash
    docker rmi -f $(docker images -aq)
    ```

- transfering files between host and container (VERY PRACTICAL)

```shell
docker cp container:path/in/container host_path
```

```shell
docker cp host_path container:path/in/container
```

- remove docker cache and danglings (can be very huge sometimes)

    ```shell
    docker system prune
    ```

- store pulled images on your machine in file for transfering

    ```bash
    docker save -o <save-location>/<file-name>.tar <image-name>
    ```

- load image from file to machine

    ```bash
    docker load -i <image-file-path>.tar
    ```

## Volumes

- always try to give name to your volumes to be easier later that you
want to delete the specific volumes.

- default volumes path is **`/var/lib/docker/volumes`**

- when you specify volumes in docker-compose below form saved in default path

    ```docker
    volumes:
        <volumename1>:
        <volumename2>:
    ```

> in docker volume mounting it is possible that
> does not work correctly and you lose your data!
> because default mode in docker volume plugging
> is `:mrw` that contains `mknod` that cause unauthorized not accessible,
> so you should specify `:rw` to remove `mknod` mode.

### Dockerfile `COPY` vs Running with `volume`

- you can use and it is recommended to use from `volume`
  for `TEST & DEVELOPMENT` BUT NOT for `PRODUCTION`.

### Dockerfile `CMD` vs docker-compose `command:`

- you can have both or just one of them
- if you have both `command:` overrides the `CMD` command 


## Network

### Default

- docker in default running usually up to one `bridge` network that has from first
  in installation.

> after installation docker your system add to a new network adapter (subnet),
> that you may see your ip in the `hostanme -I` command!

### Bridge

- while container wants to talk with each other you can
put them in a bridge network and for connection ip you can simply use
from the docker container name!
  - consider that port is original docker container port not the port-mapping
  (port-mapping is used for network host mode)

### Access to Host machien from Container

> **IN BRIDGE MODE YOU CAN ACCESS TO THE DOCKER CONTAINER FROM YOUR HOST,
> BUT YOU CANNOT ACCESS TO THE HOST FROM DOCKER CONTAINER, SO YOU SHOULD DO THIS**

- add this to `docker-compose.yaml`

    ```shell
    extra_hosts:
        - "host.docker.internal:host-gateway"
    ```

  - or run docker by flag

    ```shell
    docker run --add-host host.docker.internal:host-gateway
    ```

- now you can access to host machine from container by

    ```shell
    ping host.docker.internal
    ```

## Dockerfile

- try to always have Dockerfile in parent of all files.
- use from `Dockerfile.purpose` naming convention to be convenient! (ex: `Dockerfile.dev`)
- you cannot address the parent directory in the Dockerfile:

  ```dockerfile
  COPY ../file.txt /app/file.txt # this is wrong
  ```

- comment

    ```dockerfile
    # this is comment
    ```

- copy file from host to container

    ```dockerfile
    COPY <host-path>  <container-path>
    ```

- base image

    ```dockerfile
    FROM <image-name>:<image-version>
    ```

- set working directory of container

    ```dockerfile
    WORKDIR <path-in-container>
    ```

- container os environment variable

    ```dockerfile
    ENV <var-name>=<var-value>
    ```

- run shell command

    ```dockerfile
    RUN <shell-command>
    ```

- run command with running container

    ```dockerfile
    CMD [ '<command>', '<arg1>', '<arg2>' ]
    ```

    ```dockerfile
    ENTRYPOINT [ '<command>', '<arg1>', '<arg2>' ]
    ```

## docker-compose.yaml

```shell
docker compose up
```

> when you shut down this you just stoped it
> for removing completely you should run below:

```shell
docker compose down
```

```shell
docker compose up -d
```

```shell
docker compose down
```

> **!!!DONT WRITE THE VOLUME AND THE DOCKERFILE CONFIGS (COPY COMMANDS) IN SAME DIRECTORY!!!**
>
> this is RIDICULOUS ALTHOUGH :)

- writing `docker-compose`

```sh
# this is comment
version: <version>

networks:
    <network-name>:
        driver: <network-type>

volumes:
    <volume-name>:

services:
    <service-name>:
        image: <image-address>
        depends_on: <dependencies>
        build: <dockerfile-path>
        deploy: <deploy-config>
        ports: <port-mapping>
        restart: <restart-policy>
        networks: <networks>
        environment: <environment-variables>
        volumes: <volumes-mapping>
        container_name: <container-name>

```

## Registry

- add image to registry:

    ```bash
    sudo docker tag <image-name> <your-registry-address>/<image-name>
    sudo docker push <your-registry-address>/<image-name>
    ```

## docker swarm

- DONT USE DOCKER SWARM IF YOU CAN USE ANY OTHER APPROACHE!

> - CONNECTING 2 DOCKER ON 2 SERVER IS NOT THAT MUCH SIMPLE THAT I WAS THINKING! :/
> - first sync the dates!!!
> - SOMETIMES YOU SHOULD REMOVE THE WHOLE SWARM TO GET RID OF SOME PROBLEMS!
> - MORE THAN 2 CORE WAS REQUIRED!
> - BE CAREFUL ABOUT THE `/etc/docker/daemon.json`
> - FLAG `--detach=false` HELPS YOU TO SEE MORE LOGS!
> - i dont know why should i run this manually!: `export $(cat .env)`

- initialize the first server

```shell
docker swarm init
```

- docker swarm join

```shell
docker swarm join --token <token> <manager-ip>:2377
```

- deploy a docker compose on an network!

```shell
docker stack deploy -c docker-compose.yml my-stack
```

- check the stack status

```shell
docker stack services my-stack
```

- remove the stack

```shell
docker stack rm stack_name
```

- remove the service

```shell
docker service rm sercie_name
```

- see the logs

```shell
docker service logs service_name
```

- **SEE THE PROCESS OF SERVICE**

```shell
docker service ps service_name
```

- check swarm status (even worker node)

```shell
docker info
```

- docker all services

```shell
docker service ls
```
