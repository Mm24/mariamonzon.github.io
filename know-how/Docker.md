# Docker 

A Docker container is a file system which is created on top of the host machine. Since a container is not a virtual machine, it does not contain an own operating system but is built on top of the host's operating system. Therefore, a container can only be run on a host with the same operating system: Linux containers on Linux, Windows containers on Windows. In order to run a Linux container on Windows, Docker sets up a virtual machine running Linux, which serves as host for the containers.

An image is a blueprint for building a container. An arbitrary number of containers can be built from one image. Each image is built on top of a parent image by executing a list of commands specified in a Dockerfile.


## Dockerfile Basic Commands 
Comment lines in Dockerfiles start with #.
### FROM
The FROM (opens new window) commands specifies the parent image our image is built from. In this case, it is the shiny image from base-images directory in the BASF image directory under the URL registry.roqs.basf.net. When no such URL is specified, the default image directory is Docker Hub
(opens new window). The tag latest specifies which version of the parent image is used -- here it will be the most recent one. Other tags can be used to pull legacy images.

### EXPOSE

The EXPOSE (opens new window) command documents under which port the container communicates with outside services. The command does not actually publish the port, but only serves as documentation for the user of the image. Here the command documents that the Shiny app is to be deployed from port 5555.

### RUN 
The RUN (opens new window) command is used to start any program from the command line. Here the command is used to run an R script which installs some additional packages from CRAN.
###  ENV

The ENV (opens new window) command sets an environment variable inside the container to a specific value. Here the localization variable LC_ALL is set to a value specifying the locale to German.
### ADD

The ADD (opens new window) command is used to mount files or directories from the host system into the container. Here we mount the app folder as the directory /srv/shiny-server/app inside the container.
### CMD
In contrast to the other commands, the CMD (opens new window) command is not executed on building, but on running the container. It specifies the command to be executed inside the container when it is run. Here it is used to execute the script starting the API app. Only one CMD command can be run per container; if several are specified, only the last one will be executed.

### Further commands
See [Docker Documentation](https://docs.docker.com/engine/reference/builder/)

## Example Dockerfile
A Dockerfile specifies the commands needed for constructing a Docker image. Each line in the file specifies one build command; the commands are executed in chronological order

    ARG DOCKER_REGISTRY_PREFIX
    FROM $DOCKER_REGISTRY_PREFIX//nvidia/cuda:11.0.3-cudnn8-runtime-ubuntu20.04

    ENV DEBIAN_FRONTEND noninteractive
    # Set the user and workdir to default  folder from base image
    USER root
    WORKDIR /

    # Install tools required to build the project
    RUN apt-get update && apt-get install -y --no-install-recommends --allow-downgrades --allow-change-held-packages \
        apt-transport-https \
        apt-utils \
        build-essential \
        coreutils \
        libgl1-mesa-glx \
        libgtk2.0-dev \
        && rm -rf /var/lib/apt/lists/*

        # python3 \
        # python3-pip \
        # python3-setuptools \
        # python3-wheel \

    #----------------------------------------------------------
    # Upgrade pip and install Python packages dependencies
    #----------------------------------------------------------
    WORKDIR /tmp
    ARG PIP_INDEX_URL
    COPY requirements.txt .
    RUN python3 -m pip install --upgrade pip && \
        python3 -m pip install -r requirements.txt

    #----------------------------------------------------------
    # Copy application into the container
    #----------------------------------------------------------
    WORKDIR /code
    COPY . . 

    # Expose port
    EXPOSE 8080

    # Script to execute
    CMD ["gunicorn", "-b", "0.0.0.0:8080", "-w", "1","-t", "1200", "deploy.app:create_app()"]

