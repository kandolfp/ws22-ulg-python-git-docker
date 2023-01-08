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


# Hands on

In order to get to know Docker you need to work with it.

## Docker commands

The Docker client or CLI provides you with a multitude of commands:
```bash
> docker --help

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
To get more help with docker, check out our guides at https://docs.docker.com/go/guides/
```

Docker provides you with a nice _Getting Started_ tutorial that, no wonder, uses Docker. 

\example{
By running:
```bash
> docker run -dp 80:80 docker/getting-started
```
and than accessing [localhost](http://localhost) will give you a step by step introduction on what is happening and gives you step by step instructions to build an application in Docker.
}

In order to get a closer connection to what is happening in the rest of this course we go a different path. 
We will get a [Jupyter Lab](https://jupyter.org/) image and install additional resources as well as additional _kernels_,


# Jupyter Lab images

The Jupyter project provides us with a lot of [Docker images](https://jupyter-docker-stacks.readthedocs.io/en/latest/) on [Docker Hub](https://hub.docker.com/u/jupyter).

In order to have a bit of work to do we start with the _minimal-notebook_
By default Docker will communicate with Docker Hub so if you call 
```bash
> docker pull jupyter/minimal-notebook
Using default tag: latest
latest: Pulling from jupyter/minimal-notebook
6e3729cf69e0: Downloading [=================================>                 ]   20.3MB/30.43MB
77950dd14dd3: Download complete 
6cca258439f9: Download complete 
4f4fb700ef54: Download complete 
2e32caa4e229: Download complete 
cd2cab437071: Download complete 
7d6f92933408: Download complete 
e7f578d273e6: Download complete 
b0173d44f264: Downloading [=================>                                 ]  31.78MB/91.89MB
bd04425a2ce4: Download complete 
0580703d6738: Downloading [===================>                               ]  11.85MB/30.5MB
8213e60e2a09: Pulling fs layer 
29e508695ebe: Waiting 
9530616136f0: Waiting 
4ee249fa7dd9: Waiting 
903968ec1329: Waiting 
6cf9a67917c1: Waiting 
2bacb72b4222: Waiting 
 Digest: sha256:109283771021997caa770d367052d5c6ff640a76f8ace0bb3e60d8710fdddd8a
Status: Downloaded newer image for jupyter/minimal-notebook:latest
docker.io/jupyter/minimal-notebook:latest
```

This tells us already a lot what is happening here, we can split it in three parts.

1. `Using default tag: latest`
   
   When you store an image in a _registry_ it gets a _tag_ or _image id_, where the _tag_ is usually a human readable alias to an _image id_.
   This is very similar to the commit id in Git. 
   In fact it is again again a hash, this time with a SHA256 of the image's JSSON configuration.
1. `6e3729cf69e0: Downloading` 
   
   The next lines all look similar.
   An image ID and than the status
   - Downloading
   - Download complete
   - Pulling fs layer
   - Waiting 
   We can see that an image is not a single _file_ but consists of multiple images that in return consist of _layers_. 
   Each _layer_ is packed as an _tar_ ball (similar to _zip_). 
1. `Digest: sha256:109283771021997caa770d367052d5c6ff640a76f8ace0bb3e60d8710fdddd8a`
   
   Here, and with the additional two lines, the Docker daemon tells us what image was downloaded, from where, and if the download was a success. 

\note{
A note of caution right away. 
Please be careful when working with Docker! 
As you could see there is no need to log in to get an image and we also don't directly see what these images do.
Don't just randomly run images without considering the risks! 
When you base your image on something be careful that the source is trust worthy. 
In this case Docker Hub helps us as _jupyter_ is a verified source.
}

Now we have the image and we can run it.
Let us do so by calling:

## Running the first instance
```bash
> docker run jupyter/minimal-notebook:latest
Entered start.sh with args: jupyter lab
Executing the command: jupyter lab
[I 2023-01-05 14:33:26.282 ServerApp] jupyter_server_terminals | extension was successfully linked.
[I 2023-01-05 14:33:26.285 ServerApp] jupyterlab | extension was successfully linked.
[W 2023-01-05 14:33:26.286 NotebookApp] 'ip' has moved from NotebookApp to ServerApp. This config will be passed to ServerApp. Be sure to update your config before our next release.
[W 2023-01-05 14:33:26.286 NotebookApp] 'port' has moved from NotebookApp to ServerApp. This config will be passed to ServerApp. Be sure to update your config before our next release.
[W 2023-01-05 14:33:26.286 NotebookApp] 'port' has moved from NotebookApp to ServerApp. This config will be passed to ServerApp. Be sure to update your config before our next release.
[I 2023-01-05 14:33:26.288 ServerApp] nbclassic | extension was successfully linked.
[I 2023-01-05 14:33:26.288 ServerApp] Writing Jupyter server cookie secret to /home/jovyan/.local/share/jupyter/runtime/jupyter_cookie_secret
[I 2023-01-05 14:33:26.401 ServerApp] notebook_shim | extension was successfully linked.
[I 2023-01-05 14:33:26.512 ServerApp] notebook_shim | extension was successfully loaded.
[I 2023-01-05 14:33:26.513 ServerApp] jupyter_server_terminals | extension was successfully loaded.
[I 2023-01-05 14:33:26.513 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.10/site-packages/jupyterlab
[I 2023-01-05 14:33:26.513 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
[I 2023-01-05 14:33:26.515 ServerApp] jupyterlab | extension was successfully loaded.
[I 2023-01-05 14:33:26.517 ServerApp] nbclassic | extension was successfully loaded.
[I 2023-01-05 14:33:26.517 ServerApp] Serving notebooks from local directory: /home/jovyan
[I 2023-01-05 14:33:26.517 ServerApp] Jupyter Server 2.0.6 is running at:
[I 2023-01-05 14:33:26.517 ServerApp] http://b702c7cbc0b9:8888/lab?token=074a04006ad89060e18cfc1bb3814d323741a832d2dcaa88
[I 2023-01-05 14:33:26.517 ServerApp]  or http://127.0.0.1:8888/lab?token=074a04006ad89060e18cfc1bb3814d323741a832d2dcaa88
[I 2023-01-05 14:33:26.517 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 2023-01-05 14:33:26.519 ServerApp] 
    
    To access the server, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/jpserver-7-open.html
    Or copy and paste one of these URLs:
        http://b702c7cbc0b9:8888/lab?token=074a04006ad89060e18cfc1bb3814d323741a832d2dcaa88
     or http://127.0.0.1:8888/lab?token=074a04006ad89060e18cfc1bb3814d323741a832d2dcaa88

```
\note{The `:lates` is not required as this is the only image you have here, but to have the syntax correct from the start never hurts. }

You see a couple of log messages and at the end you are invited to access the server via `http://127.0.0.1:8888/lab?token=<token>`

If you try this you will see that the page can not be reached. 

\question{
Do you have any idea why?
}{
As mentioned earlier a container is separated from the host resources. 
In particular this means that the network is not shared. 
`127.0.0.1` is the localhost, meaning the network interface that this process has access to locally. 
There are several commands in Linux to list open ports one possibility is `netstat -lntup`

The Jupyter Lab Server tries to communicate on port `8888` but on its local host, which is not the _host machine_. 
}

In order to interface with the container we need to establish the connection to the port.

By running `docker run --help` we can have a look on at the options for the subcommand `run`.

```bash
> docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                  Add a custom host-to-IP mapping (host:ip)
  -a, --attach list                    Attach to STDIN, STDOUT or STDERR
      --blkio-weight uint16            Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device list       Block IO weight (relative device weight) (default [])
      --cap-add list                   Add Linux capabilities
      --cap-drop list                  Drop Linux capabilities
      --cgroup-parent string           Optional parent cgroup for the container
      --cgroupns string                Cgroup namespace to use (host|private)
                                       'host':    Run the container in the Docker host's cgroup namespace
                                       'private': Run the container in its own private cgroup namespace
                                       '':        Use the cgroup namespace as configured by the
                                                  default-cgroupns-mode option on the daemon (default)
      --cidfile string                 Write the container ID to the file
      --cpu-period int                 Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                  Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int              Limit CPU real-time period in microseconds
      --cpu-rt-runtime int             Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                 CPU shares (relative weight)
      --cpus decimal                   Number of CPUs
      --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                         Run container in background and print container ID
      --detach-keys string             Override the key sequence for detaching a container
      --device list                    Add a host device to the container
      --device-cgroup-rule list        Add a rule to the cgroup allowed devices list
      --device-read-bps list           Limit read rate (bytes per second) from a device (default [])
      --device-read-iops list          Limit read rate (IO per second) from a device (default [])
      --device-write-bps list          Limit write rate (bytes per second) to a device (default [])
      --device-write-iops list         Limit write rate (IO per second) to a device (default [])
      --disable-content-trust          Skip image verification (default true)
      --dns list                       Set custom DNS servers
      --dns-option list                Set DNS options
      --dns-search list                Set custom DNS search domains
      --domainname string              Container NIS domain name
      --entrypoint string              Overwrite the default ENTRYPOINT of the image
  -e, --env list                       Set environment variables
      --env-file list                  Read in a file of environment variables
      --expose list                    Expose a port or a range of ports
      --gpus gpu-request               GPU devices to add to the container ('all' to pass all GPUs)
      --group-add list                 Add additional groups to join
      --health-cmd string              Command to run to check health
      --health-interval duration       Time between running the check (ms|s|m|h) (default 0s)
      --health-retries int             Consecutive failures needed to report unhealthy
      --health-start-period duration   Start period for the container to initialize before starting health-retries countdown (ms|s|m|h) (default 0s)
      --health-timeout duration        Maximum time to allow one check to run (ms|s|m|h) (default 0s)
      --help                           Print usage
  -h, --hostname string                Container host name
      --init                           Run an init inside the container that forwards signals and reaps processes
  -i, --interactive                    Keep STDIN open even if not attached
      --ip string                      IPv4 address (e.g., 172.30.100.104)
      --ip6 string                     IPv6 address (e.g., 2001:db8::33)
      --ipc string                     IPC mode to use
      --isolation string               Container isolation technology
      --kernel-memory bytes            Kernel memory limit
  -l, --label list                     Set meta data on a container
      --label-file list                Read in a line delimited file of labels
      --link list                      Add link to another container
      --link-local-ip list             Container IPv4/IPv6 link-local addresses
      --log-driver string              Logging driver for the container
      --log-opt list                   Log driver options
      --mac-address string             Container MAC address (e.g., 92:d0:c6:0a:29:33)
  -m, --memory bytes                   Memory limit
      --memory-reservation bytes       Memory soft limit
      --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)
      --mount mount                    Attach a filesystem mount to the container
      --name string                    Assign a name to the container
      --network network                Connect a container to a network
      --network-alias list             Add network-scoped alias for the container
      --no-healthcheck                 Disable any container-specified HEALTHCHECK
      --oom-kill-disable               Disable OOM Killer
      --oom-score-adj int              Tune host's OOM preferences (-1000 to 1000)
      --pid string                     PID namespace to use
      --pids-limit int                 Tune container pids limit (set -1 for unlimited)
      --platform string                Set platform if server is multi-platform capable
      --privileged                     Give extended privileges to this container
  -p, --publish list                   Publish a container's port(s) to the host
  -P, --publish-all                    Publish all exposed ports to random ports
      --pull string                    Pull image before running ("always"|"missing"|"never") (default "missing")
      --read-only                      Mount the container's root filesystem as read only
      --restart string                 Restart policy to apply when a container exits (default "no")
      --rm                             Automatically remove the container when it exits
      --runtime string                 Runtime to use for this container
      --security-opt list              Security Options
      --shm-size bytes                 Size of /dev/shm
      --sig-proxy                      Proxy received signals to the process (default true)
      --stop-signal string             Signal to stop a container (default "SIGTERM")
      --stop-timeout int               Timeout (in seconds) to stop a container
      --storage-opt list               Storage driver options for the container
      --sysctl map                     Sysctl options (default map[])
      --tmpfs list                     Mount a tmpfs directory
  -t, --tty                            Allocate a pseudo-TTY
      --ulimit ulimit                  Ulimit options (default [])
  -u, --user string                    Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                  User namespace to use
      --uts string                     UTS namespace to use
  -v, --volume list                    Bind mount a volume
      --volume-driver string           Optional volume driver for the container
      --volumes-from list              Mount volumes from the specified container(s)
  -w, --workdir string                 Working directory inside the container
```

You have a look at 
```bash 
-p, --publish list                   Publish a container's port(s) to the host
```
and with a bit more help from the [Docs](https://docs.docker.com/engine/reference/commandline/run/) you figure out the syntax with `<port host>:<port container>` and we run:
```bash
> docker run -p 8888:8888 jupyter/minimal-notebook
```
and you have access to the `Jupyter Lab`.
\note{The Server itself does not know what port you use on your _host machine_ and therefore it will always tell you to connect to `http://127.0.0.1:8888/lab?token=<token>`, unless you tell it to change this. }

Now you can use it just like any other Jupyter lab instance with - most likely - the latest version of Python (`3.10.8` when typing these notes).

## Looking around in the container

Obviously, the first thing you want to do is to have a look and run your latest homework.
Nevertheless, checking in the _Terminal_ (you find it in the _Launcher_ under _Other_) where you currently are located with a quick `pwd` you will get the following
```bash
(base) jovyan@0648fadeef68:~$ pwd
/home/jovyan
```
Unless your parents are big fans your name is not `jovyan` and you have no user on your _host machine_ with that name. 

This is actually your container file system and user management. 
You can spot it right away in the terminal prompt. 
`<user name>@<container id>:` and you can verify it by listing all the containers currently running.
With an educated guess you try to find the command:
```bash
> docker --help | grep List
  images      List images
  port        List port mappings or a specific mapping for the container
  ps          List containers
```
and run 
```bash
docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS                    PORTS                                       NAMES
0648fadeef68   jupyter/minimal-notebook   "tini -g -- start-no…"   24 minutes ago   Up 24 minutes (healthy)   0.0.0.0:8888->8888/tcp, :::8888->8888/tcp   suspicious_solomon
```

Now you know what is happening but you still can not run your files. 
If you have a look around in the file navigation you will not get lucky either. 
Again, the container separates the file systems by default. 
But no need to despair, the file browser has an upload feature and you go for that. 

You can open your file make some changes and save them again.

\question{
What happens if we restart the container?
}{
The content inside a container is not persistent.
Meaning, if a new container is created from an image (which is done by the `docker run` command) the previous changes are no longer visible. 
}

## Bind mounting data into the container

With another educated guess you figure out the option to do a _bind mount_ inside the container.
```bash
> docker run --help | grep mount
      --mount mount                    Attach a filesystem mount to the container
  -v, --volume list                    Bind mount a volume
```
So you run your container again with
```bash
> docker run -p 8888:8888 -v "${PWD}":/home/jovyan/work jupyter/minimal-notebook
```
and when navigating to the `work` folder you see everything in the local directory (on the host) you ran the container.

Changes done to files in this directory are now also visible on the _host machine_.

\note{It is important to remember that you are nevertheless inside a container.
This means all your action - including storing data - are run by the user that is present in the container. 
```bash
(base) jovyan@82a302b74f60:~/work$ id
uid=1000(jovyan) gid=100(users) groups=100(users)
```
As a consequence, files that have not these access rights will not be shown and if you save a file it will belong to user `1000` on the _host machine_, whoever that is. 
If your _host user_ is different you can change the user inside the container by adding the option
`--user <uid>:<gid>` to the run command.
}

## Dockerfile

Now that you have all this in order you run your examples but are greeted with the response that `numpy` is not available. 
Of course you can install it in the terminal - via pip - or directly via Python but next time you start the image you need to do it again. 

So we decide to create a new image and use that instead. 
A new image is created with a `Dockerfile`, see[Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for full details. 

So we create a the file `Dockerfile` with the following content, see [Documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/recipes.html#add-a-custom-conda-environment-and-jupyter-kernel)

```Dockerfile
# Start from a core stack version
FROM jupyter/minimal-notebook:latest
# Install in the default python3 environment
RUN pip install --quiet --no-cache-dir 'numpy' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
```
1. This file first specifies that it starts working `FROM` the `jupyter/minimal-notebook:latest` image.
1. Second `pip` is used to install `numpy` and to fix some permissions that are required due to the fact that `root` is not used in this image.

You can build you own image by calling:
```bash
> docker build -t ulg22:latest .
Sending build context to Docker daemon  114.2kB
Step 1/2 : FROM jupyter/minimal-notebook:latest
 ---> f0246d6dd87f
Step 2/2 : RUN pip install --quiet --no-cache-dir 'numpy' &&     fix-permissions "${CONDA_DIR}" &&     fix-permissions "/home/${NB_USER}"
 ---> Running in 18628a817de6
Removing intermediate container 18628a817de6
 ---> 2265b09a0add
Successfully built 2265b09a0add
Successfully tagged ulg22:latest
```
and run it with
```bash
> docker run -it -p 8888:8888 ulg22
```
Now lets do something more fancy.

## Add a second kernel to the notebook

The second language uses in the class is R so why not include it into the container. 
Checking the documentation of R reveals that we need the [`IRkernel` package](https://irkernel.github.io/installation/) and the installation instructions translate to 
```Dockerfile
# Start from a core stack version
FROM jupyter/minimal-notebook:latest

RUN mamba install --quiet --yes R && \
    mamba clean --all -f -y
RUN Rscript -e "install.packages(c(\"IRkernel\"), repos = c(\"http://cran.rstudio.com\"))" && \
    Rscript -e "IRkernel::installspec()" && \
    jupyter labextension install @techrah/text-shortcuts
```

## Additional notes on Dockerfiles 

There are several more commands that can be used in the [Dockerfile](https://docs.docker.com/engine/reference/builder/):
```Dockerfile
WORKDIR /path/to/workdir
```
to set the working directory for most of the next instruction inside the image/container, 
```Dockerfile
COPY [--chown=<user>:<group>] <src>... <dest>
```
to copy files from the host file system (relative path to the context of the build) to the image/container file system (relative path to `WORKDIR`),
```Dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
```
to allow you to configure a container that will run as an _executable_, in our case the `ENTRYPOINT` is defined such that _Jupyter Lab_ starts automatically. 
Only the last specified `ENTRYPOINT` is used.
```Dockerfile
USER <user>[:<group>]
```
or
```Dockerfile
USER <UID>[:<GID>]
```
to specify the user (and group) to use for the remainder of the current stage. 
It is therefore used for the instructions `RUN`, `ENTRYPOINT`, etc.

Similar to the `.gitignore` there is also the possibility to specify a `.dockerignore` file. 
Before the Docker CLI sends the context of the build to the docker daemon (remote or local) it excludes the files and paths that are specified in this file. 
This can be used to exclude sensitive or large files from being send to the daemon.