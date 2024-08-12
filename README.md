# docker

## docker compose

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
