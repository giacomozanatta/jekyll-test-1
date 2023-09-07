---
title: Introduction 
category: 1. Docker
order: 1
---

## Install Docker Desktop
Download *Docker Desktop* from [here](https://www.docker.com/products/docker-desktop/). Then, run the installer and follows the onscreen instruction. This will install the Docker Engine, that is the core of the Docker platform, and a easy-to-use Graphical User Interface (GUI), i.e. Docker Desktop.

## The Docker Engine
The Docker Engine is shipped out with a Command-Line Iterface (CLI), and a server. The latter component runs as a root-privilege background process (daemon). You can interact with the server using the CLI client or Docker Desktop. The daemon creates and manage Docker objects, such as images and containers. 
Launching Docker Desktop will automatically start the underlying daemon process.

## How it Works
Docker is a tool that permits to run an application in an isolated environment called **container**, enabling the capability to separate applications from infrastructures. 
A container and can be seen as a sandboxed process running on a machine.
A container is created from an **image**.

# Containers

As you may have learned from the video, Docker is a tool that permits to run an application in an isolated environment, enabling the capability to separate applications from infrastructures. This environment is named **container** and can be seen as a sandboxed process running on a machine. In a single machine you can have multiple containers, each of one containing one or more applications. From docker documentation, "a container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliability from one computing environment to another". 
Docker containers are:
1. Industry standard: they could be portable anywhere.
2. Lightweight: containers does not require to run a full OS per application, since all the containers share the machine's OS system kernel.
3. Secure: containers are isolated from the overall system, and this permits to run applications in containers safety.  
![Docker container]({{ site.baseurl }}/images/docker_container.webp)
Docker automates the deployment of applications into containers, providing a set of tools and mechanism that permits to manage them.

# Images
A container is a runnable instance of an **image**. Images are the build part of Docker's lifer cycle, and they are made up of filesystems layered over each other using a union mount. Every layer of an image is mounted in read-only and is itself an image: the image below is called the parent image, where the final is called base image.
When we create a container from an image, Docker will add a read-write filesystem on top of all the image layers. In this layer runs the application that we want to "Dockerize". When Docker first start a container, this read-write layer is empty. Changes are applied only to this layer: for example, if the running application need to change a file of parent image, this file is copied into the container layer (i.e., the read-write layer), shadowing the read-only file. To better undertand this concept, just observe the image below:
![Docker image layers]({{ site.baseurl }}/images/docker_image_layers.webp)

[^1]: A daemon is a background process.