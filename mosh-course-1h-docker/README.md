# Mosh Docker Course

- what is docker so?

  - a platform for building, running and shipping applications
  - (in a consistent manner)
  - (run applications on different machine with same functionality)
  - (not more it works on my machine (consistency)(reproduce bugs!))

- reason for "it works on my machine" mistake:
  - one or more file missing
  - software engineering mismatch
  - different configuration settings

> docker solve this reasons

- when it works on your development machine
  it is definitely works on production machine

- no more extra time for config and install packages, os, dependencies, ...
  - just one command:
    ```bash
    docker-compose up
    ```
- removing previous service quickly

  - just one command:
    ```bash
    docker-compose down --rmi all
    ```

- docker is consistent in:

  - build
  - run
  - ship

- container vs virtual machines:

  - containner
    - an isolated environment for running an application
  - virtual machine
    - an abstraction of a machine (physical hardware)

- virtual machine

  - running some os on another os with sharing the hardware
  - virtual machine hosted os needs hypervisor to manage client oses
  - hypervisor examples
    - virtual box
    - VMware
    - hyper-v (just for windows)
  - in each vm we can run different frameworks and apps
    with different versions

- we have problem with virtual machiens

  1. each vm needs complete os (license, patch, ...)
  2. slow to start a complete os (os system applications, ...)
  3. resource intensive to start a complete os

- container

  - allow multiple apps in isolation
  - very very lightweight
  - use os of the host (we need just one host os)
  - starts quickly
  - need less hardware resources
  - a docker is just a process (but special kind of process!!!)

- docker architecture
  - client-server architecture
  - connection is on RESTful api
  - server also called docker engine
  - runing docker containers
  - a docker is just a process (but special kind of process!!!)
  - all containers share the os **_kernel_** host.
  - kernel manages applications and hardware resources
  - every os type (windows, mac, linux, ...) has its own kernel and so apis
  - so we cannot run windows app on linux because kernel
  - so on linux we can just run linux container
  - on windows we can run windows and also linux container (windows has some base for linux kernel (each container use from corresponding os type container))
  - mac os does not support for container but it can run 
  linux vm to run linux containers.

- install docker:
    - you should read install requirements first!
    - recommended: ***try to use from snap to install docker***
      ```
      sudo snap install docker
      ```
    - I fail very bad in other ways :)
    - installation test
      ```
      sudo docker run hello-world
      ```
      ```
      Unable to find image 'hello-world:latest' locally
      trying to download...
      ```

## Development Workflow

- we have application first, then we should dockerize it.
- for this purpose we first add a `Dockerfile` to the root 
of project.
- `Dockerfile` used to make something that we call it image.

- image:
  - a cut-down os
  - a runtime environment
  - application files
  - third-part libraries
  - environment variables

- container:
  - container is a special type of process
  - special type?
    - it has his own file system that provided in image

- run application without docker
  - one terminal:
    ```shell
    python3 manage.py runserver
    ```
  - another terminal:
    ```shell
    npm run dev
    ```
  - another terminal:
    ```shell
    systemctl start rabbitmq-server
    ```
  - ...
- run application with docker
    ```shell
    docker run <app-image>
    ```
  - after that we have an container that is image in execute

- we develop some images and we can push on some
registery server (like dockerhub, focker, ...) and 
after that everyone can use from image if he has a 
docker on his machine and run our projects with all 
requirements (with detailed dependencies)

## Docker in Action

- showing a simple typical workflow for a project:
  ```shell
  mkdir hello-docker
  cd hello-docker
  echo "console.log('Js Code Runned!')" >> app.js
  node app.js
  Js Code Runned
  ```
- how with docker:
  - create `Dockerfile`:
    ```shell
    cd hello-docker
    echo "" >> Dockerfile
    ```
  - write docker file with its specified language
  - then you should build your docker file to generate 
  a docker image.
    ```shell
    docker build -t hello-docker .
    ```
    > image not created in same directory 
    > because docker image has complex folders and files
  - `-t` means tag name should be hello-docker to run easier.
  - ensure that image created by see all images command:
    ```shell
    docker image ls
    ```
  - then you should run the image. 
    ```shell
    docker run hello-docker
    ```

# Session 2 - Linux

- distros
  - ubuntu
  - debian
  - alpine
  - fedora
  - centos
  - ...
- see docker running containers:
    ```shell
    docker ps
    ```
- see also runned history:
    ```shell
    docker ps -a
    ```
- run docker container interactive:
    ```shell
    docker run -it ubuntu
    ```
- linux is case sensitive.
- `#` means the root access (highest access)
- prompt: <current-username>@<machine-name>: <current-directory> <#/$>
- commands:
  - current-username
    ```shell
    whoami
    ```
  - return something
    ```shell
    echo "something"
    ```
  - using variable
    ```shell
    echo $variablename
    ```

## Managing Packages 

- examples of package managers
  - npm
  - yarn
  - pip
  - nuget
  - apt
  - yum
  - dnf
  - ...

- update standard apt repositories
    ```
    apt update
    ```
    