# Docker 

A Docker container is a file system which is created on top of the host machine. Since a container is not a virtual machine, it does not contain an own operating system but is built on top of the host's operating system. Therefore, a container can only be run on a host with the same operating system: Linux containers on Linux, Windows containers on Windows. In order to run a Linux container on Windows, Docker sets up a virtual machine running Linux, which serves as host for the containers.

An image is a blueprint for building a container. An arbitrary number of containers can be built from one image. Each image is built on top of a parent image by executing a list of commands specified in a Dockerfile.
## Dockerfile basics

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

