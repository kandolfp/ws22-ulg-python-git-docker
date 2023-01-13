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
  
build:
  stage: test
  before_script:
    - pip install pytest
  script:
    - pytest test.py
```
That simply executes some tests on the file `test.py` with the [pytest](https://docs.pytest.org/en/7.2.x/index.html) framework. 
