@def title = "Containerisation"
@def hascode = true

@def tags = ["containerisation, Docker"]

# Basics - why and how?

In this section we are going to talk about _containerisation_ but before we can do that let us discuss some common issues in (scientific) computing

Table of content:
\toc 

## You need a different software/library than installed

Quite often the software that is installed on a machine that you have access to or are allowed to work with does not have the specific library, software, software version that you need to work with. 
Even more often you do not have the rights to install software at your own freedom on the machine and the admin takes quite long to install what you need. 


## You need to assure reproducibility (i.e.~reproducible science)

Another frequent scenario is that you should always be able to reproduce your results. 
This means that you need to be able to run the same experiments two, three or twenty years later. 
Some reasons for this are:
- a client gets an error and you need to verify it with the version this client has
- you switch operating system
- the review process for a paper takes longer
- you need to update a library or switch to a new library and you need to verify your results
- you now work with library version _x_ but you need to regenerate some input variable that still use version _y_
- you need to make sure that on _machine a_ and _machine b_ the results are the same


## I need specific software dependencies (dependency hell)

If you haven not run into this problem by now you are either new to programming or just plain and simple lucky. 

In programming languages like Python or Julia you usually work with a multitude of libraries depending on your needs. 
It will come as no surprise to you that most of these libraries again depend on other libraries. 
This vast ecosystem is constantly growing and quickly _library a_ will depend on _library b_ with _version y_ while _library c_ needs _version z_ of _library b_. 
As you can see it is already confusing to write down so actually sorting it out on your system is even more difficult. 
While modern package managers have become better at this you will often still run into the so called _dependency hell_.  
Now consider that most likely your code will start to have these issues as well if you move from one version of a library to the next and combine it with the above paint. 

## You need to run software compiled for X but run Y

If you are running a windows machine but need to run some Linux software as there is no Windows version available. 
Or maybe you have a new Apple with an Mx chip and still want to use some software that is compiled for x86. 

## You need to test your software for X but you run Y

Maybe you are developing software on Windows but it should also run on Linux but it should also work on Android iOS and so forth.

## You have no rights to install software but you can run jobs (High Performance Computing)

When you have a massive workload that requires a multitude of CPUs, GPUs, or simply runs for quite a long time you do not want to do this on your own computer as it will limit your work experience significantly for everything else. 
Here dedicated servers or High Performance Computing infrastructure becomes useful as it can provide you with these resources. 
On most of these systems you will not be allowed to install software randomly and the admins will also not be able to provide you with every wish. 

## You need to share your software but how?

Let us assume you created some nice data processing pipeline and you want to make it available to others. 
Sure everybody can install the same environment that you have and run the code but you will get a lot of calls because something is not working. 
Often the colleges work on other operating systems and you do not really know how you can get you barely know how you got it to run on your machine. 

## You need to archive a working version (limited)

This is something that you already came across in the context of _git_ but let us consider you have some old data that can only be processed with a specific version of your software, how do you keep that software alive over a long period of time?

## Legacy code on old OS
Maybe you have some code that is only working on some old operating system but from a security point of view you can no longer run this system for your daily work and you only need it for some specific task that you need maybe once a month.

In the above scenarios (and more) virtualisation or containerisation can help. 
So what is this and what are the differences?

# Virtualisation and containerisation

Your computer or _physical machine_ is organised approximately like in the the following image:
\figenv{Organisation of process on a machine.}{/assets/pages/docker/physical_machine.svg}{}

As virtualisation and containerisation is something that lives next to this the prefix _host_ is used to make sure everybody knows what we are talking about. 

Starting from the bottom you have some hardware.
This is your CPU, various levels and layers of storage, your network device, keyboard, monitor, and so forth. 

The _host kernel_ is mainly the operating system. 
It provides you with a standardized interface to the hardware so you do not need to use different programs depending on the brand of monitor you use. 

The _host process_ is what you mainly interface with on a computer.
This could be MS Word, you favourite internet browser or music player, in short applications are made up of processes. 

## Virtualisation
Now you can run some virtualisation on your machine.
As the name suggests it will provide a virtual environment. 
For example [Virtual Box](https://www.virtualbox.org/) is a widely used way to bring Linux to your Windows machine or the other way around.
This way you only need set of hardware but have the possibilities of different operation systems available to you. 
This looks something like the following:
\figenv{Organisation of process within a virtual machine.}{/assets/pages/docker/virtual_machine.svg}{}

From the view of the _host_ the entire virtualisation is one (or more) _guest processes_, illustrated above by the \color{#087f5b}{green box}. 

The _\color{#e67700}{hypervisor}_ is translating between the _host kernel_ and the virtual hardware that is needed for the _\color{#e67700}{guest kernel}_.
This means, the _guest kernel_ is seeing its own set of hardware (CPU, GPU, Keyboard, and so forth). 
Quite often you can for example decide if a connected USB Drive is used by the guest or the host. 
On top of the _hypervisor_ everything is the same as on the host. 
This also means you use up resources to provide this. 

## Containerisation
Now containerisation is a operating system based virtualisation. 

\figenv{Organisation of process within a container.}{/assets/pages/docker/container_machine.svg}{}
You have a specific _\color{#e67700}{runtime}_ but not a second kernel. 
The runtime takes care of the separation on the level of namespaces. 
We only discuss Linux containers in this detail and do not guarantee that Windows containers use a different mechanism. 
Namespaces are a mechanism of the kernel that separates or _partitions_ resources such that different processes use different set of resources. 
For examples, the container might only see 4 of your 16 CPUs, no network drive, can only use 2 GB of main storage and so forth. 
This separation also makes sure that there is no leakage of information from one process to the other. 
You can also have a different set of rights or run as a different user. 
Other than with a virtualisation the _host kernel_ is used and also the _host hardware_ if is allowed via the namespace configuration. 

# Different container solutions
The containerisation started with [Docker](https://docs.docker.com/). 
Nowadays, there are a different solutions available with a different focus and different set of functionalities:
1. [Open Container Initiative](https://opencontainers.org/) for example [Podman](https://podman.io/)
   - open industry standards for container format & runtime
   - Allows for a daemon-less container engine for running OCI containers (root or rootless)
1. [AppTainer](https://apptainer.org/) - formally know as Singularity.
   - designed for HPC
   - uses Linux namespaces but leaves resource limitations to the batch system
   - feels like running a program
   - allows access to _all_ host resources
   - runs in the user namespace
1. [Docker](https://docs.docker.com/)
   - based on Linux namespaces and resource limitations
   - client server model (_dockerd_)
   - typical _microservices_ (see [docker-compose](https://docs.docker.com/compose/) for a swarm)
   - very popular with a huge collection on dockerhub

If you want to fully understand what a container actually is it is very instructive to have a look at this video by [Liz Rice](https://www.lizrice.com/) _Learn how to code a container using Go code_
~~~
<iframe width="560" height="315" src="https://www.youtube.com/embed/8fi7uSYlOdc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
~~~

Our focus will be on Docker. 