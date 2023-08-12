---
title: Containers
category: 1. Docker
order: 2
---
# What is a Container?
<mark>A container is a sandboxed process running on a host machine that is isolated from all other processes running on that host machine</mark>. That isolation leverages kernel namespaces and cgroups, features that have been in Linux for a long time. Docker makes these capabilities approachable and easy to use. To summarize, a container:

Is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI.
Can be run on local machines, virtual machines, or deployed to the cloud.
Is portable (and can be run on any OS).
Is isolated from other containers and runs its own software, binaries, configurations, etc.
If you’re familiar with chroot, then think of a container as an extended version of chroot. The filesystem comes from the image. However, a container adds additional isolation not available when using chroot.

![](https://www.docker.com/wp-content/uploads/2021/11/container-vm-whatcontainer_2.png)

# What is an image?
A running container uses an isolated filesystem. This isolated filesystem is provided by an image, and the image must contain everything needed to run an application - all dependencies, configurations, scripts, binaries, etc. The image also contains other configurations for the container, such as environment variables, a default command to run, and other metadata.
