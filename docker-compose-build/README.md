# Docker Compose Build

## Table of Contents
- [Docker Compose Build](#docker-compose-build)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Using Compose to build](#using-compose-to-build)
  - [Tutorial](#tutorial)

## Overview 
This tutorial helps building new images instead of using a predefined one. 

## Using Compose to build

- Compose can also build your custom images
- Will build them with docker-compose up if not found in cache
- Also rebuild with docker-compose build
- Great for complex builds that have lots of vars or build args

## Tutorial
To use docker-compose to build follow the steps bellow. We're gonna use a simple NGINX server to server some HTML files as a sample. 

1. Create an nginx.Dockerfile with the following content:

    ```docker
    FROM nginx:1.13

    COPY nginx.conf /etc/nginx/conf.d/default.conf
    ```
2. Create a nginx.conf with the following content:

    ```nginx
    server {

      listen 80;

      location / {

        proxy_pass         http://web;
        proxy_redirect     off;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Host $server_name;

      }
    }
    ```
3. Create a docker-compose.yml with the following content:

    ```docker
    version: '2'

    services:
      proxy:
        build:
          context: .
          dockerfile: nginx.Dockerfile
        image: nginx-custom
        ports:
          - '80:80'
      web:
        image: httpd
        volumes:
          - ./html:/usr/local/apache2/htdocs/
    ```
4. Create an html folder inside your project root folder and put some html files there. Or instead you just can copy the ones in this tutorial folder. 
5. Run docker compose:

    ```bash
    docker-compose up --build
    ```
6. Open the html files on your browser, e.g. http://localhost
   