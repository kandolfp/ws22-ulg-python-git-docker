@def title = "Docker"
@def hascode = true

@def tags = ["containerisation, Docker, basics"]

# Disclaimer

These notes are a mix of different sources: 
- [Docker docs](https://docs.docker.com/get-started/overview/)

# Basics of Docker

Docker understands itself as an _open platform for developing, shipping, and running applications_. 
By separating the application form the infrastructure it allows you to develop independent of the platform the _container_ is running on later. 

The _docker platform_ allows you to run many applications in isolated environments in so called _containers_. 
This allows you to split up your application into different containers, for example the database is separate from your application and so forth. 
This allows you to update different parts independently from each other and revert changes if required. 
This approach is often summed up under the term _micro services_.

Docker uses a specific architecture to achieve this:
\figenv{The docker architecture.}{/assets/pages/docker/docker_architecture.svg}{}

Mainly docker consists of a client `docker` and server `dockerd` (docker daemon) approach. 

## The Docker daemon
The Docker daemon is in charge of all the Docker objects such as images, containers, networks, volumes and listens for API calls. 
Furthermore, it is able to communicate with other daemons.

## The Docker client
The docker client is your way to interact with Docker. 
Instructions like `docker pull` are sent as an API call to `dockerd` and executed there. 
The daemon that you communicate with does not necessarily be on the local machine nor does it always have to be the same.

## The Docker registries
The Docker registries are separate and strictly speaking not necessary for your to use Docker. 
There purpose is to store Docker images. 
This can be public like [Docker Hub](https://hub.docker.com/) and [Autamus](https://autamus.io/registry/)[^1] or private for your organisation and tools like GitLab provide registries as well.

[^1]: A Semi-Autonomous Build System for Scientific Containers

When you use a command like `docker pull` you pull an image from a registry and with `docker push` you push it to a registry, the same as with Git. 

Again, the client can talk to multiple registries. 

The various docker objects in the above image are

## Images

> An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

In order to build you own image you need a so called `Dockerfile` which uses a specific syntax to allow the Docker daemon to build an image from it. 
Each step in this image is called a layer and whenever you build an image only the layers that change need to be rebuild. 

## Container
> A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

The container is isolated from the host machine due to the use of _namespaces_ but you can allow interaction with the host machine like opening a network port or _mounting_ a directory into the image. 

> A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.

## Docker commands

\example{
The `docker` client or CLI provides you with a multitude of commands:
```bash
$ docker --help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/home/c102338/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/c102338/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/c102338/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/c102338/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.9.1)
  compose*    Docker Compose (Docker Inc., v2.11.1)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```
To get more help with docker, check out our guides at https://docs.docker.com/go/guides/
}