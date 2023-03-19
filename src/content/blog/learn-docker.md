---
author: Aman Mavai
pubDatetime: 2023-03-19T15:22:00Z
title: Learn Docker
postSlug: learn-docker
featured: true
draft: false
tags:
  - docker
  - containers
ogImage: ""
description: Learn all about docker.
---

# Concepts

`- Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications.`

## **Docker architecture**

`Docker Engine` is a client-server application with these major components:

> A server which is a type of long-running program called a daemon process (the dockerd command).

> A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

> A command line interface (CLI) client (the docker command).

#

> Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting
> of building, running, and distributing your Docker containers.

> The Docker client and daemon can run on the same system,  
> or you can connect a Docker client to a remote Docker daemon.  
> The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface.

![architecture](../img/architecture.svg)

## The Docker daemon

The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers,
networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

## The Docker client

The Docker client (`docker`) is the primary way that many Docker users interact with Docker. When you use commands such
as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker
API. The Docker client can communicate with more than one daemon.

## Docker registries

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to
look for images on Docker Hub by default. You can even run your own private registry. If you use Docker Datacenter
(DDC), it includes Docker Trusted Registry (DTR).

When you use the `docker pull` or `docker run` commands, the required images are pulled from your configured registry.
When you use the docker push command, your image is pushed to your configured registry.

## Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This
section is a brief overview of some of those objects.

## **_IMAGES_**

An image is a _read-only_ template with instructions for creating a Docker container. Often, an image is based on
another image, with some additional customization. For example, you may build an image which is based on the ubuntu
image, but installs the Apache web server and your application, as well as the configuration details needed to make your
application run.

You might create your own images or you might only use those created by others and published in a registry. To build
your own image, you create a `Dockerfile` with a simple syntax for defining the steps needed to create the image and run
it. Each instruction in a `Dockerfile` creates a layer in the image. When you change the `Dockerfile` and rebuild the
image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and
fast, when compared to other virtualization technologies.

## **_CONTAINERS_**

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the
Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image
based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how
isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host
machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it.
When a container is removed, any changes to its state that are not stored in persistent storage disappear.

### **Containerization**

```
The use of containers to deploy applications is called containerization. Containers are not new, but their use for easily deploying applications is.
```

## Images and containers

Fundamentally, a `container` is nothing but a running process, with some added encapsulation features applied to it in
order to keep it isolated from the host and from other container.

One of the most important aspects of `container` isolation is that each `container` interacts with its own private
filesystem; this filesystem is provided by a Docker image. An image includes everything needed to run an application -
the code or binary, runtimes, dependencies, and any other filesystem objects required.

### **Container vs Virtual Machine**

![container-vs-vm](../img/container-vs-vm.PNG)

### **Container & Virtual Machine**

![container-&-vm](../img/container-and-vm.PNG)

### **Port Mapping or Port Publishing**

![port-mapping](../img/port-mapping.PNG)

### **Volume Mapping**

![volume-mapping](../img/volume.PNG)

### **Managing Data in Docker**

Docker has two options for containers to store files in the host machine, so that the files are persisted even after the
container stops: **`volumes`**, and **`bind mounts`**. If you’re running Docker on Linux you can also use a tmpfs mount.
If you’re running Docker on Windows you can also use a named pipe.

![types-of-mounts](../img/types-of-mounts.png)

**`Volumes`** are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on
Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in
Docker.

**`Bind mounts`** may be stored anywhere on the host system. They may even be important system files or directories.
Non-Docker processes on the Docker host or a Docker container can modify them at any time.

`tmpfs mounts` are stored in the host system’s memory only, and are never written to the host system’s filesystem.

# Commands

```javascript
$ docker --version

// Test Docker installation
$ docker run hello-world

// runs the container based of mentioned image, if doesn't exist, it downloads from registry and then run it.
$ docker run <image-name>

 // list running containers
$ docker ps

// list all containers
$ docker ps -a

// stop container
$ docker stop <image-name>

// remove a stopped/exited container
$ docker rm <continer-name/container-id>

// The --force option stops a running container, so it can be removed.
// If you stop the container running with docker stop bb first, then you do not need to use --force to remove it.
$ docker rm --force bb

// list images
$ docker images

// remove images, ! Delete all dependent containers to remove image
$ docker rmi <image-name>

// download an image
$ docker pull <image-name>

// append a command, when we run the container
$ docker run ubuntu sleep 5

// execute a command on a running container
$ docker exec <container-name> cat etc/hosts

// detach
$ docker run -d <image-name>

// attach
$ docker attach <container-id>

// tag, if none specified, default is latest.
$ docker run <image-name:tag>

// ------------------------------------------------------------
// By default docker containers doesn't listen to std-input
// it runs in a non-interactive mode

// -i parameter is for interactive mode
// -t option to attach to container's terminal
$ docker run -it kodekloud/simple-prompt-docker

// Port Mapping or Port publishing, you cannot add a port mapping while the container is running
$ docker run -p host-port:container-port <image-name>

// Lets see how data is persisted in docker-container
$ docker run -v /opt/datadir:var/mysql mysql

// get more details of container
$ docker inspect <container-id>

// get logs of the container
$ docker logs <container-id>

// build an image from Dockerfile
$ docker build --tag <tag-namme> folder-containing-Dockerfile

// build an image from Dockerfile ; Name and optionally a tag in the 'name:tag' format
$ docker build -t NAME:TAG folder-containing-Dockerfile

// --name specifies a name with which you can refer to your container in subsequent commands, in this case bb
$ docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0

// docker run most things covered, 3000:80(host-port:container-port), --rm(remove the container once it stops), -d(detach mode)
$ docker run -p 3000:80 -d --name container-app-name --rm image-name

// find basic info about your docker configuration
$ docker info

// to see how the image was built
$ docker history <image>

// stop all containers
$ docker container prune

// remove all images
$ docker image prune

// remove all volumes
$ docker volume prune

// remove all networks
$ docker network prune
```

> A Docker image is a **private file system** just for your container. It provides all the _files and code_ your
> container needs.

> Dockerfiles describe how to assemble a private filesystem for a container, and can also contain some metadata
> describing how to run a container based on this image.

## Sample Dockerfile

Writing a Dockerfile is the first step to containerizing an application. You can think of these Dockerfile commands as a
step-by-step recipe on how to build up your image. The Dockerfile in the bulletin board app looks like this:

```Dockerfile
# Use the official image as a parent image.
FROM node:current-slim

# Set the working directory.
# To specify that all subsequent actions should be taken from the directory /usr/src/app in your image filesystem (never the host’s filesystem).
WORKDIR /usr/src/app

# Copy the file from your host to your current location.
# COPY the file package.json from your host to the present location (.) in your image (so in this case, to /usr/src/app/package.json)
COPY package.json .

# Run the command inside your image filesystem.
RUN npm install

# Inform Docker that the container is listening on the specified port at runtime.
# EXPOSE 8080 in the Dockerfile in the end is optional. It documents that a process in the container will expose this port. But you still need to then actually expose the port with -p when running docker run. So technically, -p is the only required part when it comes to listening on a port. Still, it is a best practice to also add EXPOSE in the Dockerfile to document this behavior.
EXPOSE 8080

# Run the specified command within the container. This instruction is not executed when the image is created but when a container is started based on the image.
CMD [ "npm", "start" ]

# Copy the rest of your app's source code from your host(same folder that contains your Dockerfile) to your image filesystem.
COPY . .
```

### The dockerfile defined in this example takes the following steps:

- Start `FROM` the pre-existing node:current-slim image. This is an official image, built by the node.js vendors and
  validated by Docker to be a high-quality image containing the Node.js Long Term Support (LTS) interpreter and basic
  dependencies.
- Use `WORKDIR` to specify that all subsequent actions should be taken from the directory /usr/src/app in your **image
  filesystem** (never the host’s filesystem).
- `COPY` the file package.json from your host to the present location (.) in your image (so in this case, to
  /usr/src/app/package.json)
- `RUN` the command npm install inside your **image filesystem** (which will read package.json to determine your app’s
  node dependencies, and install them)
- `COPY` in the rest of your app’s source code from your host to your image filesystem.
- `CMD` if you dont specify a CMD , CMD of the base image will be executed. with no base image and no CMD you would get
  an error.

> You can see that these are much the same steps you might have taken to set up and install your app on your host.
> However, capturing these as a Dockerfile allows you to do the same thing inside a portable, isolated Docker image.

> The steps above built up the filesystem of our image, but there are other lines in your Dockerfile.

> The CMD directive is the first example of specifying some metadata in your image that describes how to run a container
> based on this image. In this case, it’s saying that the containerized process that this image is meant to support is
> npm start.

> The EXPOSE 8080 informs Docker that the container is listening on port 8080 at runtime.

## Images and Layers

A Docker image is built up from a series of `layers`. Each layer represents an `instruction` in the image’s Dockerfile.
Each layer except the very last one is `read-only`. Consider the following Dockerfile:

```Dockerfile
FROM ubuntu:18.04
LABEL org.opencontainers.image.authors="org@example.com"
COPY . /app
RUN make /app
RUN rm -r $HOME/.cache
CMD python /app/app.py
```

This Dockerfile contains four commands. Commands that modify the filesystem create a layer.

The `FROM` statement starts out by creating a layer from the ubuntu:18.04 image.

The `LABEL` command only modifies the image’s metadata, and does not produce a new layer.

The `COPY` command adds some files from your Docker client’s current directory. The first `RUN` command builds your
application using the make command, and writes the result to a new layer.

The second `RUN` command removes a cache directory, and writes the result to a new layer.

Finally, the `CMD` instruction specifies what command to run within the container, which only modifies the image’s
metadata, which does not produce an image layer.

Each layer is only a set of differences from the layer before it. Note that both adding, and removing files will result
in a new layer. In the example above, the $HOME/.cache directory is removed, but will still be available in the previous
layer and add up to the image’s total size.

The layers are stacked on top of each other. When you create a `new container`, you add a `new writable layer` on top of
the underlying layers. This layer is often called the `“container layer”`. All changes made to the running container,
such as writing new files, modifying existing files, and deleting files, are written to this
`thin writable container layer`. The diagram below shows a container based on an ubuntu:15.04 image.

![imagesandlayers](../img/container-layers.jpeg)

## Containers and Layers

The major difference between a container and an image is the `top writable layer`. All writes to the container that add
new or modify existing data are stored in this writable layer. When the container is deleted, the writable layer is also
deleted. The underlying image remains unchanged.

Because each container has its own writable container layer, and all changes are stored in this container layer,
`multiple containers can share access to the same underlying image and yet have their own data state`. The diagram below
shows multiple containers sharing the same Ubuntu 15.04 image.

![containersandlayers](../img/sharing-layers.jpeg)

## Volumes

Volumes are `folders on your host machine hard drive` which are `mounted` ("made available", mapped) into containers.

Volumes persist if a container shuts down. if a container restarts and mounts a volume, any data inside of that volume
is available in the container.

A Container can write data into a volume and read data from it.

Volumes that get created from docker file VOLUME instruction are anonymous volumes. It only exists as long as our container exists.

We can't create `named volumes` inside of a docker file. instead we have to create a named volume when we run a
container.

Anonymous volumes are closely attached to a specific container. Named volumes are not attached to a container.

If there are clashes , the longer internal path wins

## Removing Anonymous Volumes

We saw, that anonymous volumes are **removed automatically**, when a container is removed.

This happens when you start / run a container with the `--rm` option.

If you start a container **without that option**, the anonymous volume would **NOT be removed**, even if you remove the
container (with `docker rm ...`).

Still, if you then re-create and re-run the container (i.e. you run `docker run ...` again), a `new anonymous volume`
will be created. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a
different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a
new one).

Now you just start **piling up a bunch of unused anonymous volumes** - you can clear them via
`docker volume rm VOL_NAME` or `docker volume prune`.

# COPY vs ADD
COPY is Same as 'ADD' but without the tar and remote url handling.


# Networks
- Sending requests from inside your container to the world wide web just works out of the box.
- From docker container to host machine when we use `host.docker.internal` as address
- For container to container communication, it requires a container network + use container name as address
