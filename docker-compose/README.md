# Docker Compose

## Table of Contents
- [Docker Compose](#docker-compose)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [docker-compose.yml](#docker-composeyml)
  - [docker-compose CLI](#docker-compose-cli)
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

## References
- [Docker Compose Release for Linux](https://github.com/docker/compose/releases)
- [YAML Official Site](https://yaml.org/)
- [YAML Reference Card](https://yaml.org/refcard.html)
- [YAML to JSON](https://nodeca.github.io/js-yaml/)
