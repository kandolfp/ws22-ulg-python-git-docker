@def title = "cicd"
@def hascode = true

@def tags = ["cicd"]

# Basics - why and how?

The term _CI/CD_ is most commonly attributed to _continuous integration, continuous delivery, and continuous deployment_. 
By introducing automation and continuous monitoring of the code lifecycle - from integration and testing to delivering and deploying - it helps keep the you codebase working and our _product_ up and running. 

The concept is often used in DevOps, MLOps or similar approaches. 

## Continuous integration

The main idea of _continuous integration_ is to automate building and testing such that your merge to a shared branch or repository is known to work. 
The idea is that multiple people can work on the same code base and conflicts get recognized early and not after two months of developing. 
Typically the CI _pipeline_ does unit and integration tests that make sure that the chances have not broken the code. 
This allows for regular, hopefully daily, merge pushes to the shared repository. 

In Git therms this would be a common _remote repository_ for the developers. 

## Continuous delivery

Now that you know that the code is automatically build and unit as well as integration tested the automated delivery to a shared repository is the next step.

This means your code base is always ready to be deployed to a production environment, meaning that it is not just used by you but by others and even other programs build on it. 

## Continuous deployment

The last step in this automation pipeline is the automatic deployment of the code to a production environment where it can be used.

## An example
Let us use these notes as example to illustrate this process:

1. The authors write some new material about e.g. CI/CD stuff. This might take some time until the pictures are ready, the text has the correct format and code blocks are tested and developed.
1. In the next step the new section is _commited_ to the local repository. This goes on for a couple of commits and separate for each author. 
1. After the author is happy with the local changes it pushes the commits to the _remote repository_. 
1. The repository living on [github](https://github.com/kandolfp/ws22-ulg-python-git-docker) uses [github workflows](https://docs.github.com/en/actions/using-workflows) to 
   - Checkout the repository
   - Test if the page can be build, if not tell the author
   - If the build was successful the page is moved over to [github pages](https://pages.github.com/)
1. The reader of the notes always has the latest working copy of the notes. 

\note{Obviously, this does not prevent the notes from having some errors or typos but the authors always get a quick feedback if there work can still be displayed.}

# CI/CD in GitLab

GitLab supports and provides some tools for [CI/CD](https://docs.gitlab.com/ee/ci/). 

The main part is a CI/CD pipeline that is by default triggered when a `.gitlab-ci.yml` file is present in a directory. 

Let us consider the following example:
```yml
stages:
  - test
  
image: python:3.11

variables:
  VAR1: "true"
  
run_test:
  stage: test
  before_script:
    - pip install pytest
  script:
    - pytest test.py
```
That simply executes some tests on the file `test.py` with the [pytest](https://docs.pytest.org/en/7.2.x/index.html) framework. 

What is actually happening in that little yml file is the as follows:

1. Definition of the stages to run, here only one called `test`
1. Definition of the default Docker image to use for the stages
1. Definition of some variables, only dummies in this case
1. the actual stage called `run_test` with
   - before script to install the `pytest` package and all other requirements
   - the actual test run

The entire job is executed on a so called [runner](https://docs.gitlab.com/runner/). In the case of the used GitLab you have a public runner available to handle the jobs. 
You can also just spin up a Docker container on your laptop as a runner, attach it and use it for the pipeline. 
As no _triggers_ are specified the pipeline is executed on every push to the remote repository. 
In the GitLab UI you can find the CI/CD section.

## Set up your own GitLab runner within Docker

This description is mainly based on [GitLab docs](https://docs.gitlab.com/runner/install/docker.html) and we focus on a Linux installation. 

First we need to make sure that we have `docker` installed on the machine we want to run the GitLab runner on, see the [Docker](../docker/index) section for some basic information. 
As the runner is using docker itself for executing our CI/CD pipeline we will need to make sure that we have access to the Docker socket inside the `gitlab-runner` container. 
We can do this by mapping `/var/run/docker.sock` to the container, note the `-v` option later on. 

The runner requires a configuration and it should be not inside the container as this would not make it permanent. Go to the directory of the GitLab repository that you want to create the runner for, obviously you can use a shared directory but sometimes it is good to have to runner configuration in this directory, and create the directory `./gitlab-runner/config/`.

GitLab provides a runner on Docker-Hub and we can simple use it, it is called `gitlab/gitlab-runner`. 
As the runners should be forward and backward compatible with various GitLab versions we work with the `latest` tag.

In order to do the configuration you need to talk to the gitlab-runner application inside the container. 
This looks like:
```bash
docker run --rm -t -i gitlab/gitlab-runner --help

NAME:
   gitlab-runner - a GitLab Runner

USAGE:
   gitlab-runner [global options] command [command options] [arguments...]

VERSION:
   15.9.1 (d540b510)

AUTHOR:
   GitLab Inc. <support@gitlab.com>

COMMANDS:
   exec                  execute a build locally
   list                  List all configured runners
   run                   run multi runner service
   register              register a new runner
   reset-token           reset a runner's token
   install               install service
   uninstall             uninstall service
   start                 start service
   stop                  stop service
   restart               restart service
   status                get status of a service
   run-single            start single runner
   unregister            unregister specific runner
   verify                verify all registered runners
   artifacts-downloader  download and extract build artifacts (internal)
   artifacts-uploader    create and upload build artifacts (internal)
   cache-archiver        create and upload cache artifacts (internal)
   cache-extractor       download and extract cache artifacts (internal)
   cache-init            changed permissions for cache paths (internal)
   health-check          check health for a specific address
   read-logs             reads job logs from a file, used by kubernetes executor (internal)
   help, h               Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --cpuprofile value           write cpu profile to file [$CPU_PROFILE]
   --debug                      debug mode [$RUNNER_DEBUG]
   --log-format value           Choose log format (options: runner, text, json) [$LOG_FORMAT]
   --log-level value, -l value  Log level (options: debug, info, warn, error, fatal, panic) [$LOG_LEVEL]
   --help, -h                   show help
   --version, -v                print the version
```

What we will need is the `register` command and make sure that the _result_ is available for the container afterwards, so map the directory we created inside the container. 
Note that we can get the required URL and token form GitLab by going to the _Settings->CI/CD->Runners_ and _Specific Runners_. 
Furthermore, for _executor_ select _docker_.

```bash
docker run -rm -t -i -v ./gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register

Runtime platform            arch=amd64 os=linux pid=7 revision=d540b510 version=15.9.1
Running in system-mode.                            
                                                   
Enter the GitLab instance URL (for example, https://gitlab.com/):
https://git.uibk.ac.at/
Enter the registration token:
ImNotTellingYouMyToken
Enter a description for the runner:
[1fc67126c305]: ULG
Enter tags for the runner (comma-separated):
ulg
Enter optional maintenance note for the runner:

WARNING: Support for registration tokens and runner parameters in 
the 'register' command has been deprecated in GitLab Runner 15.6 and 
will be replaced with support for authentication tokens. 
For more information, see https://gitlab.com/gitlab-org/gitlab/-/issues/380872 
Registering runner... succeeded       runner=ImNotTellingYouMyToken
Enter an executor: parallels, ssh, kubernetes, instance, custom, docker, 
docker-ssh, shell, virtualbox, docker+machine, docker-ssh+machine:
docker
Enter the default Docker image (for example, ruby:2.7):
python:3.11
Runner registered successfully. Feel free to start it, but if it's running already 
the config should be automatically reloaded!
 
Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml" 
```
Let us have a look at the config file:
```toml
concurrent = 1
check_interval = 0
shutdown_timeout = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "ULG"
  url = "https://git.uibk.ac.at/"
  id = 304
  token = "ImNotTellingYouMyToken"
  token_obtained_at = 2023-03-13T17:20:31Z
  token_expires_at = 0001-01-01T00:00:00Z
  executor = "docker"
  [runners.cache]
    MaxUploadedArchiveSize = 0
  [runners.docker]
    tls_verify = false
    image = "python:3.11"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
```
Now if you want to have a `dind` (Docker in Docker) image running inside your container you need to make sure that docker is available, inside the docker image that is executed in the gitlab-runner docker image. 
Again we _simply_ need to forward the docker from the host machine and this can be done via the `config.toml` with the `volume` key.

```toml
  [runners.docker]
    tls_verify = false
    image = "python:3.11"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
    shm_size = 0
    pull_policy = "if-not-present"
```
If you also add the `pull-policy = "if-not-present"` you will be able to use an image build with a `dind` in one stage as the base of a second stage. 

We can finally put all together and start the runner in a daemon mode that will always restart:
```bash
mkdir -p /gitlab-runner/config
docker run -d --name gitlab-runner --restart always \
  -v ./gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

\note{
1. To make the runner accept jobs without a tag you need to specifically allow this. In the GitLab project, _Settings->CI/CD->Runners_ and _Specific Runners_ you will find the runner and a little edit possibility.
Simply check the _Run untagged jobs_  box
1. It might be that the _Shared Runner_ is preferred for a none tagged job, you can deactivate it in  _Settings->CI/CD->Runners_ and _Shared Runners_
1. The runner will use the resources of the infrastructure you installed it on. As a side effect you will see all the docker images used pop up on this machine. 
1. If you want to shut the runner down again use: `docker rm gitlab-runner`, if you just use `kill` it will automatically restart.
}

Let us consider the following example:
```yaml
stages:
  - build
  
variables:
  docker_image: "ulg:latest"
 
run_build:
  stage: build
  tags:
    - ulg
  image: docker:dind
  before_script:
    - docker images
  script:
    - docker build -t "$docker_image" .

run_test:
  stage: build
  tags:
    - ulg
  image:
    name: $docker_image
    entrypoint: [""]
  script:
    - R --version
  rules:
    - when: on_success

```

The main idea of this pipeline is to build a docker image and than test it in the next step. 
For the test we use a an image that specified in the `Dockerfile` in [Add a second kernel to the notebook](../docker/basics#add_a_second_kernel_to_the_notebook).
We have a single stage that runs two jobs (`build` and `test`), this makes sure that they run in sequence.
The _global_ variable `docker_image` is used to define the image name that should be build and tested. 

In the first part `run_build` we use the _Docker in Docker_ `docker:dind` image to actually build the image, see the variable `image`. 
The script to do the actual build is than simple. 

In the second part `run_test` we use the build image as the base for the job.
As this image is automatically starting a jupyter notebook we need to override the entrypoint, see [Docker](../docker/basics#additional_notes_on_dockerfiles). 
We just tell the image to do nothing, that way the `script` section will take place, where we simply check the version of `R`.

\note{
1. The additional `rule` is there to make sure that this part is only run for a successful build before
1. The `tags` make sure that the desired runner is used
1. The runner is configured with the additional setup described above. 
}