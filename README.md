# Docker

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
    docker run <image-name>
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

> in docker volume mounting it is possible that
> does not work correctly and you lose your data!
> because default mode in docker volume plugging
> is `:mrw` that contains `mknod` that cause unauthorized not accessible,
> so you should specify `:rw` to remove `mknod` mode.

## Network

### Bridge

- while container wants to talk with each other you can
put them in a bridge network and for connection ip you can simply use
from the docker container name!
  - consider that port is original docker container port not the port-mapping
  (port-mapping is used for network host mode)

### Access to Host machien from Container

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

- comment

    ```shell
    # this is comment
    ```

- copy file from host to container

    ```shell
    COPY <host-path>  <container-path>
    ```

- base image

    ```shell
    FROM <image-name>:<image-version>
    ```

- set working directory of container

    ```shell
    WORKDIR <path-in-container>
    ```

- container os environment variable

    ```shell
    ENV <var-name>=<var-value>
    ```

- run shell command

    ```shell
    RUN <shell-command>
    ```

- run command with running container

    ```shell
    CMD [ '<command>', '<arg1>', '<arg2>' ]
    ```

    ```shell
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
