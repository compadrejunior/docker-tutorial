# Docker Compose

## Table of Contents
- [Docker Compose](#docker-compose)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [docker-compose.yml](#docker-composeyml)
  - [docker-compose CLI](#docker-compose-cli)
  - [Using NGINX as a Proxy to Apache](#using-nginx-as-a-proxy-to-apache)
  - [References](#references)

## Overview
Docker Compose is a tool to facilitate creating multiple docker images through a single yaml file. 

Why use Docker Compose: 
- Configure relationships between containers
- Save our docker container run settings in easy-to-read file

Comprised of 2 separate but related things:
1. YAML-formatted file that describes our solution options for:
   - Containers
   - Networks
   - Volumes
2. A CLI took **docker-compose** used for local dev/test automation with those YAML files

## docker-compose.yml

- Compose [YAML](https://yaml.org/) format has it's own versions: 1, 2, 2.1, 3, 3.1
- YAML file can be used with docker-compose command for local docker automation
- Can be used with docker directly in production with [Swarm](https://www.techtarget.com/searchitoperations/definition/Docker-Swarm#:~:text=Docker%20Swarm%20is%20a%20clustering,the%20OS%20and%20container%20images.) (as of v1.13)
- docker-compose --help
- docker-compose.yml is default filename, but any can be used with docker-compose -f 


- Template:

    ```YAML
    version: '3.1' # if no version is specified then v1 is assumed. Recommended v2 minimum

    services: # containers. same as docker run
      servicename: # a friendly name. This is also DNS name inside network
        image: # Optional if use build
        command: # Optional, replace the default CMD specified by the image
        environment: # Optional, same as -e in docker run
        volumes: # Optional, same as -v in docker run
      servicename2: 
      
    volumes: # Optional, same as docker volume create

    networks: # Optional, same as docker network create
    ```

## docker-compose CLI
- CLI tool comes with Docker for Windows/Mac, but separate download for Linux
- Not a production-grade tool but ideal for local development and test
- Two most common commands are:
  ```bash 
  docker-compose up # setup volumes/networks and start all containers
  docker-compose down # stop all containers and remove cont/vol/net
  ```

- If all projects had a Dockerfile and docker-compose.yml then "new developer onboarding" would be:
  ```bash 
  git clone github.com/some/software
  docker-compose up
  ```

## Using NGINX as a Proxy to Apache
The YAML file tells docker-compose to start two new Containers (proxy and web). 
```YAML
version: '3'

services: 
  proxy:
    image: nginx:1.11
    ports:
      - '80:80'
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
  web:
    image: httpd
```

The Proxy service is a nginx container which will serve in port public 80 and redirect all requests to the Web service (an apache http container)

The Volumes configuration maps the local nginx.conf to the container directory in read only mode. 

```bash
server {

  listen 80;

  location / {

    proxy_pass http://web;
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Host $server_name;

  }
}
```

The Web service is a simple Apache HTTP server listening in private port 80.

Docker compose will generate a default network for both containers to communicate. 

The following steps will guide you to run the containers:

1. Running the docker-compose up command.
    ```bash
    docker-compose up
    ```

2. Docker will bring both container logs in foreground.
    ```bash
    Creating network "docker-compose_default" with the default driver
    Creating docker-compose_proxy_1 ... done
    Creating docker-compose_web_1   ... done
    Attaching to docker-compose_web_1, docker-compose_proxy_1
    web_1    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.3. Set the 'ServerName' directive globally to suppress this message
    web_1    | AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.19.0.3. Set the 'ServerName' directive globally to suppress this message
    web_1    | [Sat Jun 18 22:06:43.923712 2022] [mpm_event:notice] [pid 1:tid 139989106617664] AH00489: Apache/2.4.53 (Unix) configured -- resuming normal operations
    web_1    | [Sat Jun 18 22:06:43.923807 2022] [core:notice] [pid 1:tid 139989106617664] AH00094: Command line: 'httpd -D FOREGROUND'
    ```
3. Open the browser an type localhost. The log will be updated showing that the request was captured by NGINX proxy and redirected to Apache. 

    ```bash
    proxy_1  | 192.168.56.1 - - [18/Jun/2022:22:09:02 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.5005.124 Safari/537.36 Edg/102.0.1245.44" "-"    
    ```
4. Press CTRL+C to quit the containers. 
5. Run the docker-compose command again with -d option to send the output to the background. 
    ```bash
    docker-compose up -d
    ```
6. Run docker-compose log to check the logs.
    ```bash
    docker-compose logs
    ```
7. To see the list of containers running use the following command.
    ```bash
    docker-compose ps
    ```
8. To check the list or processes running inside the containers, use the following command. 
    ```bash
    docker-compose top
    ```
9.  To stop and remove all containers and resources, use the following command. 
    ```bash
    docker-compose down
    ```

## References
- [Docker Compose Release for Linux](https://github.com/docker/compose/releases)
- [YAML Official Site](https://yaml.org/)
- [YAML Reference Card](https://yaml.org/refcard.html)
- [YAML to JSON](https://nodeca.github.io/js-yaml/)
