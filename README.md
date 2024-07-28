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
