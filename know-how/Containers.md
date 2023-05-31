# Docker 

A Docker container is a file system which is created on top of the host machine. Since a container is not a virtual machine, it does not contain an own operating system but is built on top of the host's operating system. Therefore, a container can only be run on a host with the same operating system: Linux containers on Linux, Windows containers on Windows. In order to run a Linux container on Windows, Docker sets up a virtual machine running Linux, which serves as host for the containers.

An image is a blueprint for building a container. An arbitrary number of containers can be built from one image. Each image is built on top of a parent image by executing a list of commands specified in a Dockerfile.

## [Installation](https://docs.docker.com/engine/install/ubuntu/)

``` bash
$ sudo apt-get update && apt-get install \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

#  Add Docker’s official GPG key:
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up the repository:
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  (lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install docker engine
$sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

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

## Docker Registries
It is usually advisable to use base images provided on dedicated servers, so-called Docker Registries. 
The most important external registry (and the default registry used by Docker) is Docker Hub, provided by Docker, Inc., the creators of the Docker software. 

The Registry is a stateless, highly scalable server side application that stores and lets you distribute Docker images. The Registry is open-source, under the permissive Apache license. You can find the source code on GitHub.


# Singularity

Singularity is a container platform that allows software engineers and researchers to easily share their work with others by packaging and deploying their software applications in a portable and reproducible manner. When you download a Singularity container image, you essentially receive a virtual computer disk that contains all of the necessary software, libraries and configuration to run one or more applications or undertake a particular task, e.g. to support a specific research project. This saves you the time and effort of installing and configuring software on your own system or setting up a new computer from scratch, as you can simply run a Singularity container from the image and have a virtual environment that is identical to the one used by the person who created the image. Container platforms like Singularity provide a convenient and consistent way to access and run software and tools. Singularity is increasingly widely used in the research community for supporting research projects as it allows users to isolate their software environments from the host operating system and can simplify tasks such as running multiple experiments simultaneously.

You may be familiar with Docker, another container platform that is now used widely. If you are, you will see that in some ways, Singularity is similar to Docker. However, in others, particularly in the system’s architecture, it is fundamentally different. These differences mean that Singularity is particularly well-suited to running on distributed, High Performance Computing (HPC) infrastructure, as well as a Linux laptop or desktop!

## [Installation](https://docs.sylabs.io/guides/3.0/user-guide/installation.html#install-on-linux)


``` bash
$ sudo apt-get update && sudo apt-get install -y \
    build-essential \
    libssl-dev \
    uuid-dev \
    libgpgme11-dev \
    squashfs-tools \
    libseccomp-dev \
    pkg-config
    
 # Install Go and set up environment
 $ export VERSION=1.11 OS=linux ARCH=amd64 && \
    wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
    sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
    go get -u github.com/golang/dep/cmd/dep && \
    rm go$VERSION.$OS-$ARCH.tar.gz
    
$ echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
    echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
    source ~/.bashrc

$ sudo apt-get install -y singularity-container
``` 
