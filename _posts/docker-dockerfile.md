




## Dockerfile

To build an image a set of instructions are used that are called Dockerfile. Those are layered steps that docker execute one-by-one to create an deployable image.

## ENTRYPOINT VS CMD

Basicaly those are commands that are executed when container is started. The difference is that ENTRYPOINT can't be replaced with *run IMAGE CMD* command. They can be used both. 

Example:
```
IMAGE1
CMD ["echo"]
```
Running IMAGE1 with *docker run IMAGE1 "text"* will give us an error.
```
IMAGE2
ENTRYPOINT ["echo"]
```
Running IMAGE2 with *docker run IMAGE2 "text"* will give us *text* in the terminal.

Those 2 instruction can be used together. Like this:
```
IMAGE3
ENTRYPOINT ["echo"]
CMD ["text1"]
```
And if executed with *docker run IMAGE3" will give us *text1* but if executed with *docker run IMAGE3 text2* will give us *text2*.

It is very important to know that in this case we can't access container on the start  by giving parameters *-it IMAGE sh*. To do this we can execute *exec -it IMAGE sh* if container is running or overwrite the ENTRYPOINT with --entrypoint param, like this *run --entrypoint sh IMAGE*. But this can't work in some cases.

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

## Docker additive layers

Layers are additive. This means that during build process deleting files won't save any memory, image won't be lighter because layers have weight. Layers that added files will still contain them. 

The solution is to delete files in the layer that they are added/created.

Example: If some packages are built and temp files are stored in cache that has to emptied we can delete it in the same layer.

Docker allows us to use string/chain commands to execute them in one layer.

Example (Fedora): <i style="color:orange">RUN dnf install httpd -y && dnf clean install</i>