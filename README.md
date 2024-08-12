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

- remove docker cache and danglings (can be very huge sometimes)

    ```shell
    docker system prune
    ```

## Network

- how to access to host machien from container:

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
docker compose up -d
```

```shell
docker compose down
```

```sh
# this is comment
version: <version>

networks: <networks-define>

volumes:
    <volume-name>:
        driver: <volume-type>

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
