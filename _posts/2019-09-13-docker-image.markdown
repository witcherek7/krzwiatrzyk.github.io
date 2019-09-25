---
layout: post
title: Dockerfile breakdown
date: 2019-09-07 8:00:00 +0100
description: What docker Image is and what instructions are used to build it.
img: 
fig-caption: # Add figcaption (optional)
tags: [Docker]
---

## Docker commands cheat sheet

+ `docker export`  - export container files into the specific path 
<br>
Example: <i>`docker export dfs1233dsf -o application.tar`</i>

## What Docker Image is?

Docker Image is a basic construct of application. Docker images provide the basis for everything that you will ever deploy and run with Docker. 

## Dockerfile

To build an image a set of instructions are used that are called Dockerfile. Those are layered steps that docker execute one-by-one to create an deployable image.

## Dockerfile instructions

+ `FROM` is a statement importing an image. Building an application always start with import base images. They can be viewed on Docker Hub.
<br>
Example: <i>`FROM node:11.11.0`</i>

+ `LABEL` is used to add metadata via key and value pairs. Used for additional information about image
<br>
Example: <i>`LABEL "developer"="kwiatrzy"`</i>
<br>
<br>
To show image labels type `docker inspect` command.

+ `USER` can be used to change with which user processes should be run. By default Docker runs all processess as root.
<br>
Example:<i> `USER kwiatrzy`</i>

+ `ENV` instruction allows to set shell variable that can be used later in process of building image.
<br>
Example:<i> `ENV APPATH /data/app`</i>

+ `ADD` command copies files from local filesystem or a remote URL into the image.
<br>
Example:<i> `ADD ./app /app`</i>

+ `WORKDIR` changes working directory for the remaining build instruction. 
<br>
Example:<i> `WORKDIR /app`</i>

+ `CMD` defines the command that launches the process in the container.
<br>
Example: <i>`CMD ["python","run.py"]`</i>


## Dockerfile basic information

Dockerfile is built by set of instruction in specific order. This order has high impact on build times. Instructions that changes a lot should be placed at the bottom of the Dockerfile.

## Docker Intermediate containers

While building docker image we can encounter an error that could stop bulding an image. In that situation it is easy to troubleshoot because docker is building an intermediate container for every step in Dockerfile. We can get into that container and check why the heck it causing us an error. Great!


