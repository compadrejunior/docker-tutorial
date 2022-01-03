# Docker Tutorial

This repo contains Docker basic tutorials and sample files for beginners.

## Overview

These steps will guide you in simple Docker tasks for development and administration. To know more about docker follow the link https://docs.docker.com/get-started/overview/

## Install

Install docker following the steps from https://docs.docker.com/get-docker/

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

## Docker help

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
- Other components: many other tools will be described used as necessary in this tutorial but, for the sake of clarity, we will only mention them when needed.

## Container Commands

These are basic container commands for pulling, running and stopping a container based on a popular existing image.

### Starting a new Container

This will start a NGINX container for the first time.

```bash
docker run --publish 80:80 nginx
```

**Explaining**: docker will check if an image of name nginx exist locally and download if it does not. It will routes the traffic from your local machine port 80 to the container port 80. If it succeed the container will run and you can test you application by typing http://localhost on your browser, otherwise, you get an error informing any problem such as bind error (if your local port is already in use).

To detach from the container terminal just press Ctrl+C. If it fails, you can list the running containers and stop it or run the container in the detached mode as described in the following topics.

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

The CONTAINER ID is the short reference used to stop, start and manage the container. The NAMES field will list the names assigned to the container. They can also be used to reference the container. If you do not specify a name on the docker run command, as we did previously, it will generate a random name. Eg. youthful_wozniak. You can specify a name with the option --name on the docker run command.

😄 **Fun facts**: youthful_wozniak is a reference to [Steve Wozniak](https://en.wikipedia.org/wiki/Steve_Wozniak). Docker will create a a new random container name, honoring engineers and scientists, every time you run a docker run without specifying a name.

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

## Stopping a container

To stop a container, use the docker stop command following by its NAME or CONTAINER ID. See the examples bellow:

```bash
docker stop youthful_wozniak
```

or

```bash
docker stop cc3a0dc43716
```

## Running a container with a detached terminal

The following command will run the container and immediately return to the local terminal prompt.

```bash
docker run --publish 80:80 --detached nginx
```

Sample output:

```bash
c3ef23e3e9a92828de363d7869f6043769ec27d046d503ba679a3ac6f4ac2285
```

The output is a full CONTAINER ID reference code.

## Running an existing container

When you stop a container by running the docker stop command, it will not destroy the existing container but instead it will stop it (putting it in a stopped state). So if you run docker run again it will try to create a new container and fail if the port is already in use or succeed if no other containers are running on the assigned port.

The right way to start a stopped container is by running the docker start command, following by the container NAME or CONTAINER ID.

Fo example:

```bash
docker start youthful_wozniak
```

## Docker Desktop GUI

The Docker Desktop GUI can also be used to manage, start and stop containers On Windows and Mac. See the following links:

- [Docker Desktop for Window](https://docs.docker.com/desktop/windows/)
- [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/)
