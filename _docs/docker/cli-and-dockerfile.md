---
title: 3. CLI and Dockerfile
category: 1. Docker
order: 3
---
<h2>Contents</h2>
* toc
{:toc}
In the previous sections, we talk about Docker and we saw how we can download an image and launch a container.  
Here, we are going to explain how to interact with the Docker daemon using the Docker CLI and how we can create our custom images.

## The Docker CLI
The Docker Engine is shipped out with a docker CLI client application.
You can interact with docker using the *docker* command:

{% highlight bash %}
~/Projects/sw-arch/docker docker

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Log in to a registry
  logout      Log out from a registry
  search      Search Docker Hub for images
  version     Show the Docker version information
  info        Display system-wide information

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.11.2-desktop.1)
  compose*    Docker Compose (Docker Inc., v2.20.2-desktop.1)
  container   Manage containers
  context     Manage contexts
  dev*        Docker Dev Environments (Docker Inc., v0.1.0)
  extension*  Manages Docker extensions (Docker Inc., v0.2.20)
  image       Manage images
  init*       Creates Docker-related starter files for your project (Docker Inc., v0.1.0-beta.6)
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  plugin      Manage plugins
  sbom*       View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc., 0.6.0)
  scan*       Docker Scan (Docker Inc., v0.26.0)
  scout*      Command line tool for Docker Scout (Docker Inc., 0.20.0)
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Swarm Commands:
  swarm       Manage Swarm

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes

Global Options:
      --config string      Location of client config files (default "/Users/giacomo/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket to connect to
  -l, --log-level string   Set the logging level ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/Users/giacomo/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/Users/giacomo/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/Users/giacomo/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Run 'docker COMMAND --help' for more information on a command.

For more help on how to use Docker, head to https://docs.docker.com/go/guides/
{% endhighlight %}

Docker CLI commands are self-explanatory: hovewer, we will see briefly the most important ones.
### Show all the downloaded images
You can get a list of all the images that you have downloaded with the command **docker images** (or **docker image ls**):
{% highlight bash %}
~/Projects/sw-arch/docker docker images
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
golang                   latest    57ca605b665e   12 hours ago   814MB
httpd                    latest    7860e7628717   31 hours ago   168MB
docker/getting-started   latest    3e4394f6b72f   8 months ago   47MB
{% endhighlight %}
### Download an image from the Docker Hub
What about downloading an image? Just type <b>docker pull *&lt;name-of-image:tag&gt;*</b> (example: **docker pull nodejs:latest**).  
Tag permits to specify which version of the Docker image you want to use. If you don't specify a tag, the latest available image will be pulled.
You can see all the available tags for an image directly from the Docker Hub (for example, for node: <a href="https://hub.docker.com/_/node/tags">hub.docker.com/_/node/tags</a>).
{% highlight bash %}
~/Projects/sw-arch/docker docker pull node:latest
latest: Pulling from library/node
012c0b3e998c: Already exists 
00046d1e755e: Already exists 
9f13f5a53d11: Already exists 
e13e76ad6279: Pull complete 
95103e803d28: Pull complete 
c3ef23edee6c: Pull complete 
cde810d34647: Pull complete 
cfeacc2c3f89: Pull complete 
Digest: sha256:69cf8e7dcc78e63db74ca6ed570e571e41029accdac21b219b6ac57e9aca63cf
Status: Downloaded newer image for node:latest
docker.io/library/node:latest

What's Next?
  View summary of image vulnerabilities and recommendations → docker scout quickview node:latest
~/Projects/sw-arch/docker docker images
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
node                     latest    add6f751ed2b   11 hours ago   1.1GB
golang                   latest    57ca605b665e   13 hours ago   814MB
httpd                    latest    7860e7628717   32 hours ago   168MB
docker/getting-started   latest    3e4394f6b72f   8 months ago   47MB
~/Projects/sw-arch/docker 
{% endhighlight %}
As we mention before, an image is composed of layers, that are images themselves. When you download an image, you download the layers that compose that image and this is clearly visible by the logs of the docker pull command (i.e., 012c0b3e998c, 00046d1e755e, 9f13f5a53d11 are the first three layers that compose this image). This permits to avoid redownloading a layer if you have already pulled it for another image. On the example above, we have that the first three layers already exists in local, so Docker skips the download of these layers.  

The downloaded image is then visible inside Docker Desktop:
![Docker images]({{ site.baseurl }}/images/docker_cli_1.png)


## The Dockerfile