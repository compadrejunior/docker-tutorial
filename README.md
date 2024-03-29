# Docker Tutorial

This repo contains Docker basic tutorials and sample files for beginners.

## Table of Contents

- [Docker Tutorial](#docker-tutorial)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Install](#install)
  - [Basic commands](#basic-commands)
    - [Docker version](#docker-version)
    - [Docker info](#docker-info)
    - [Docker help](#docker-help)
  - [Docker Basic Components](#docker-basic-components)
  - [Container Commands](#container-commands)
    - [Starting a new Container](#starting-a-new-container)
    - [Starting a specific version of an image](#starting-a-specific-version-of-an-image)
    - [Listing running containers](#listing-running-containers)
    - [Specifying a name for the container](#specifying-a-name-for-the-container)
    - [Listing all containers](#listing-all-containers)
    - [Stopping a container](#stopping-a-container)
    - [Running a container with a detached terminal](#running-a-container-with-a-detached-terminal)
    - [Running an existing container](#running-an-existing-container)
    - [Viewing the container logs](#viewing-the-container-logs)
    - [Listing the processes inside a container](#listing-the-processes-inside-a-container)
    - [Safely deleting a container](#safely-deleting-a-container)
  - [Docker Desktop GUI](#docker-desktop-gui)
  - [References](#references)
  - [More Topics](#more-topics)

## Overview

These steps will guide you in simple Docker tasks for development and administration. To know more about docker, check the [Docker Overview](https://docs.docker.com/get-started/overview/) documentation.

## Install

Install docker following the steps from [Get Docker](https://docs.docker.com/get-docker/) page.

## Basic commands

Following is a list of important basic commands for Docker:

### Docker version

Informs which versions of Docker Client and Docker Engine are installed on your local machine.

```bash
docker version
```

Sample output:

```bash
Client:
 Cloud integration: v1.0.22
 Version:           20.10.11
 API version:       1.41
 Go version:        go1.16.10
 Git commit:        dea9396
 Built:             Thu Nov 18 00:42:51 2021
 OS/Arch:           windows/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.11
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.9
  Git commit:       847da18
  Built:            Thu Nov 18 00:35:39 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.12
  GitCommit:        7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc:
  Version:          1.0.2
  GitCommit:        v1.0.2-0-g52b36a2
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

### Docker info

This command outputs detailed information about your docker environment.

```bash
docker info
```

Sample output:

```bash
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc., v0.7.1)
  compose: Docker Compose (Docker Inc., v2.2.1)
  scan: Docker Scan (Docker Inc., v0.14.0)

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 20.10.11
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc version: v1.0.2-0-g52b36a2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 5.10.16.3-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 16
 Total Memory: 24.98GiB
 Name: docker-desktop
 ID: PUX6:N7TG:4WFX:KLDT:XSTV:QHX5:YCEO:DC35:NE5A:NSUS:ISBM:2BVB
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
```

### Docker help

To see a list of common usage of docker CLI use the following command:

```bash
docker --help
```

## Docker Basic Components

Docker is composed of many components which will be needed to run, manage, pull and publish images and containers:

- Image: is a copy of an application, operation system or service that was publish in a public registry, such as Docker Hub, or your company private registry.
- Container: container is an instance of an image that is running on the host machine. One image can have multiple instances of the same container.
- Registry: is a private or public service to publish, download and manage images.
- Dockerfile: a file used to create a new container image.
- Other components: many other tools will be described in this tutorial but, for the sake of clarity, we will only mention them when needed.

## Container Commands

These are basic container commands for pulling, running and stopping a container based on a popular existing image. E.g. NGINX.

### Starting a new Container

This will start a NGINX container for the first time.

```bash
docker run --publish 80:80 nginx
```

**Explaining**: When you run the docker run command we just ran, the following will happen:

- Docker will check if an image of name nginx exist locally.
- If it doesn't find one it it will be downloaded from the registry. In our case, Docker Hub.
- It will check if the specified version exists in Docker Hub. Since we didn't specify one, it will get the latest version.
- Creates the container and prepare to start.
- Gives it a virtual IP on a private network inside docker engine.
- It will routes the traffic from your local machine port 80 to the container port 80. If it succeed the container will run and you can test you application by typing `http://localhost` on your browser.
- Start the command defined in the CMD in the image Dockerfile.

If you already have an application or another container running on port 80, you will get an error a bind error message:

```bash
b0d0ff929f3b1b9813261484262649fad7a4c89cdfff781876a1da05d8a61398
docker: Error response from daemon: driver failed programming external connectivity on endpoint youthful_wozniak (2b47264c3cd97d7443411be979c60e490b40721f157926c1f217255435f1556f): Bind for 0.0.0.0:80 failed: port is already allocated.
```

To detach from the container terminal just press Ctrl+C. If it fails, you can list the running containers and stop it or run the container in the detached mode as described in the following topics.

### Starting a specific version of an image

You can specify the version of the image when running a new container.

You can check available versions in the public registry, for instance Docker Hub.

For the NGINX application you chan check the [Docker Hub NGINX Image](https://hub.docker.com/_/nginx) page to see a list of available versions and some valuable instructions.

Bellow we have an example in how to specify the version of an image by tag value after the name, separated by colon character.

```bash
docker run --publish 80:80 nginx:1.20.2
```

Remind that the value after the colon can be any tag such as latest, for the most recent version or stable for a version that is unchanged and working.

### Listing running containers

To view a list of running containers type the following:

```bash
docker ps
```

Sample output:

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                NAMES
cc3a0dc43716   nginx     "/docker-entrypoint.…"   45 minutes ago   Up 2 minutes   0.0.0.0:80->80/tcp   youthful_wozniak
```

The CONTAINER ID is the short reference used to stop, start and manage the container. The NAMES field will list the names assigned to the container. They can also be used to reference the container. If you do not specify a name on the docker run command, as we did previously, it will generate a random name. Eg. youthful_wozniak.

😄 **Fun facts**: youthful_wozniak is a reference to [Steve Wozniak](https://en.wikipedia.org/wiki/Steve_Wozniak). Docker will create a a new random container name, honoring engineers and scientists, every time you run a docker run without specifying a name.

### Specifying a name for the container

You can specify a name with the option --name on the docker run command.

```bash
docker run --publish 80:80 --detach --name nginx nginx
```

If the name is already in use, you will get the following error message:

```bash
docker: Error response from daemon: Conflict. The container name "/nginx" is already in use by container "359841bdab5b89041d212f50a2feaf052ea84f2cf86f729c785db8a1ae3751c7". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
```

### Listing all containers

To see a list of all local containers run the following command:

```bash
docker container ls --all
```

Sample output:

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS                         PORTS                NAMES
c3ef23e3e9a9   nginx     "/docker-entrypoint.…"   About an hour ago   Exited (0) 59 minutes ago                           lucid_hoover
f61f3b04b56b   nginx     "/docker-entrypoint.…"   About an hour ago   Exited (0) About an hour ago                        clever_hopper
cc3a0dc43716   nginx     "/docker-entrypoint.…"   3 hours ago         Up 59 minutes                  0.0.0.0:80->80/tcp   youthful_wozniak
```

### Stopping a container

To stop a container, use the docker stop command following by its NAME or CONTAINER ID. See the examples bellow:

```bash
docker stop youthful_wozniak
```

or

```bash
docker stop cc3a0dc43716
```

### Running a container with a detached terminal

The following command will run the container and immediately return to the local terminal prompt.

```bash
docker run --publish 80:80 --detached nginx
```

Sample output:

```bash
c3ef23e3e9a92828de363d7869f6043769ec27d046d503ba679a3ac6f4ac2285
```

The output is a full CONTAINER ID reference code.

### Running an existing container

When you stop a container by running the docker stop command, it will not destroy the existing container but instead it will stop it (putting it in a stopped state). So if you run docker run again it will try to create a new container and fail if the port is already in use or succeed if no other containers are running on the assigned port.

The right way to start a stopped container is by running the docker start command, following by the container NAME or CONTAINER ID.

Fo example:

```bash
docker start youthful_wozniak
```

### Viewing the container logs

To see the log for a container run docker container logs command following by the container name.

```bash
docker container logs nginx
```

### Listing the processes inside a container

To see a list of running processes inside a container use the command docker container top following by its name.

```bash
docker container top nginx
```

### Safely deleting a container

To safely delete a container, run the docker container rm command informing the container name.

```bash
docker container rm nginx
```

Notice that, if the container is running, Docker will prevent it from deletion.

Sample output:

```bash
Error response from daemon: You cannot remove a running container 359841bdab5b89041d212f50a2feaf052ea84f2cf86f729c785db8a1ae3751c7. Stop the container before attempting removal or force remove
```

## Docker Desktop GUI

The Docker Desktop GUI can also be used to manage, start and stop containers On Windows and Mac wit a friendly user interface. See the following links:

- [Docker Desktop for Window](https://docs.docker.com/desktop/windows/)
- [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/)

## References

- [udemy-docker-mastery](https://github.com/BretFisher/udemy-docker-mastery)

## More Topics

For more tutorials, check the folders inside this repo.
