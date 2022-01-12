# CLI Process Monitoring

- [CLI Process Monitoring](#cli-process-monitoring)
  - [Intro](#intro)
  - [Listing the running containers](#listing-the-running-containers)
  - [Viewing the processes inside a container](#viewing-the-processes-inside-a-container)
  - [Checking The Container Details](#checking-the-container-details)
  - [Monitoring Resource Usage](#monitoring-resource-usage)

## Intro

This tutorial shows how to monitors your containers through the command line.

First, we need to start a few containers

Lets run a NGINX server container:

```bash
docker container run -d --name nginx nginx
```

> The -d option will run the container in the detached mode.

Then we will start a MySQL database:

```bash
docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
```

> The -e will set environment variables and pass it to the container.

## Listing the running containers

To see the list of running containers use the following command

```bash
docker container ls
```

Sample output:

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                 NAMES
539f1ef27375   mysql     "docker-entrypoint.s…"   8 seconds ago   Up 6 seconds   3306/tcp, 33060/tcp   mysql
b43ef0c927b3   nginx     "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes   80/tcp                nginx
```

## Viewing the processes inside a container

The **top** command will show the processes running inside a container. Try the following:

```bash
docker container top mysql
```

Sample output:

```bash
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
999                 1657                1638                1                   01:12               ?                   00:00:00            mysqld
```

```bash
docker container top nginx
```

Sample output:

```bash
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                1201                1182                0                   01:09               ?                   00:00:00            nginx: master process nginx -g daemon off;
uuidd               1264                1201                0                   01:09               ?                   00:00:00            nginx: worker process
uuidd               1265                1201                0                   01:09               ?                   00:00:00            nginx: worker process
```

## Checking The Container Details

With can inspect the container properties with the following command:

```bash
docker container inspect mysql
```

Sample output:

```bash
[
    {
        "Id": "539f1ef27375a4cfe0a43ed930c52d7270ed69769ee257a56e86097fca79ddaf",
        "Created": "2022-01-12T01:12:34.862111Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mysqld"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1657,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-01-12T01:12:35.2884685Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b",
        "ResolvConfPath": "/var/lib/docker/containers/539f1ef27375a4cfe0a43ed930c52d7270ed69769ee257a56e86097fca79ddaf/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/539f1ef27375a4cfe0a43ed930c52d7270ed69769ee257a56e86097fca79ddaf/hostname",
        "HostsPath": "/var/lib/docker/containers/539f1ef27375a4cfe0a43ed930c52d7270ed69769ee257a56e86097fca79ddaf/hosts",
        "LogPath": "/var/lib/docker/containers/539f1ef27375a4cfe0a43ed930c52d7270ed69769ee257a56e86097fca79ddaf/539f1ef27375a4cfe0a43ed930c52d7270ed69769ee257a56e86097fca79ddaf-json.log",
        "Name": "/mysql",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
...
```

The resulting text is in JSON format.

> A lot of information is displayed in the output. You can use the `| more` command to scroll the results if you need.

## Monitoring Resource Usage

You can check how the container is using the host resources like memory, CPU, network and I/O, just by typing:

```bash
docker container stats
```

Sample output:

```bash
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O   PIDS
539f1ef27375   mysql     0.14%     397.5MiB / 24.98GiB   1.55%     1.08kB / 0B   0B / 0B     37
b43ef0c927b3   nginx     0.00%     16.48MiB / 24.98GiB   0.06%     1.6kB / 0B    0B / 0B     17
```
