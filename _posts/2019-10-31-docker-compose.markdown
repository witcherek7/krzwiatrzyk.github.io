---
layout: post
title: Docker-compose breakdown
date: 2019-10-16 8:00:00 +0100
description: Docker-compose
img: 
fig-caption: # Add figcaption (optional)
tags: [Docker]
---

## What it is?

Docker compose is a tool realased by Docker Inc. to easily run and connect bunch of containers.

## Docker Compose file

Docker Compose is declared in YAML file, docker-compose.yaml.

## File components

Version:
```
version: '2'
```

Network:
```
networks:
  mynetwork:
    driver: bridge
```

Services:
```
services:
  service1:
    build:
      context: ../../files/docker     # context shows where files for building an image are
    image: ubuntu
    restart: unless-stopped
    command: sh
    volumes:
      - "localfolder:/data/containerfolder"
    networks:
      - mynetwork
    depends_on:
      - service2    #informs Docker-compose that service2 has to be started before starting service 1
    ports:
      - 3000:3000
    expose: 
      - "80"     #expose port to other containers in the network but not for the host
    environments:
      ROOT_URL: "blblabla"    #we can pass environmental variables into container

```


`../..` - two folder up

## Commands

To check if configuration is correct:
```
docker-compose config
```
This will print any errors if there is an mistake in docker-compose.yaml file.


To build the containers from file:
```
docker-compose build
```

Starting containers:
```
docker-compose up -d
```

To see logs:
```
docker-compose logs
```

To restart one container:
```
docker-compose restart NAME
```

To see an overview of containers and processes that are running in them:
```
docker-compose top
```

Execute command in container:
```
docker-compose exec NAME bash
```

To start/stop etc:
```
docker-compose start/stop/pause/unpause NAME
```

To stop and remove containers:
```
docker-compose down
```



