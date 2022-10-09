@def title = "Git - Basics"
@def hascode = true

@def tags = ["git, basics"]


# Basics of Git
Git is a distributed VCS, in this case this means the entire _repository_ is distributed on various machines and possible multiple _remotes_. 
This was a clear design choice that allows individual contributors to work independently of the availability of a _remote_ but still have the full history available. 

In particular, such a setup could look something like this:
\figenv{Basic setup}{/assets/pages/git/distributed1.svg}{}

In this setup the _Remote Repository_ is the place you send your changes to in order to have them visible for others, and in return you can get changes from them via the _Remote Repository_. 

Like the name suggests the _Local (development) environment_ sits on your machine. 
The _working directory_ is your current version of the files contained in the repository and the _Local Repository_ is the copy of the entire repository (with all changes) on your machine. 
We will learn more about these parts as we go along. 

## Let us start with getting a _Remote Repository_

In order to allow a playfield for this class I created a repository that we are going to use. 
In order to get it onto you local machine type the following commands in the Terminal:

```bash
#Navigate to a suitable directory
git clone https://{YOUR ID}@git.uibk.ac.at/c102338/ulg22_playground.git
```
This will perform the following two actions:
1. _Checkout_ the content of the _remote repository_ into the _\color{#e67700}{working directory}_. By default this is the name of the repository, in your case the folder `ulg22_playground` is created and all files are put there. 
1. A copy of the _remote repository_ is stored in the _\color{#1864ab}{local repository}_. For all intended purposes, it acts exactly the same as the _Remote Repository_, with the sole exception that is not shared with others. 

\figenv{Clone a remote repository to your machine}{/assets/pages/git/distributed2.svg}{}

## Adding content

With the following snippet you can view the content of the repository (the second line is the response):
```bash 
> ls ulg22_playground
python_ex1 README.md
```
As you can see there is currently only a `README.md` and the folder `python_ex1`. 

Now lets add our solutions of the exercises we did in Python to the repository. 

First we copy the file to exercise directory
```bash
cp PATH/TO/YOUR/FILE/solution_ex1.py python_ex1/{YOUR ID}.py 
```
In my case the command reads like this:
```bash
> cp ../Exercixes/reference_solution.py python_ex1/ID.py
'../Exercixes/reference_solution.py' -> 'python_ex1/ID.py'
```
Now we also add the file to the table in the `README.md` with your favourite editor (with correct markdown syntax):
```markdown
# ulg22_playground

## List of handin the python exercises 1 

| Name/UID    | File        |
| ----------- | ----------- |
| ID     | [my upload](python_ex1/ID.py) |
```
This has we modified our _working directory_. In order to get an idea what Git thinks about this lets run `git status` in the working directory:
```bash
> git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	python_ex1/ID.py

no changes added to commit (use "git add" and/or "git commit -a")

```
Lets break down the output of the command.
First, Git tells you on what _branch_ you are on (we will hear more about that later), second, that _local repository_ is different from the _remote repository_ and it states that you have _tracked_ and _untracked_ files. 

Now, a _tracked_ files is a file that is part of the repository and Git is keeping _track_ of what is happening to it. 
An _untracked_ file on the other hand, is a file that is in the same directory but it is not managed by Git. 

Git tells you how to change the status of your _untracked_ file into a _tracked_ file. 

We do this by running `git add python_ex1/ID.py`.

Now it is time to introduce another Git concept, namely the _\color{#862e9c}{staging area}_. 
The stating area is the curious white spot  between your _working directory_ and the _local repository_ in the above pictures. 
This is the place where Git collects all the changes to your files that you want to put into the _local repository_.
\figenv{Staging area with the changes that can be moved to the repository}{/assets/pages/git/staging.svg}{}

Now we also want to add the changes in `README.md` and therefore we run `git add README.md`.

By rerunning `git status` we get
```bash
> git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md
	new file:   python_ex1/ID.py

```

Now that we are satisfied that all our changes are in the _staging area_ we are ready to _commit_ the changes to the _local repository_. 
This is done by running `git commit`. 
A text editor will open and you are able to write a message telling everybody what you just did in the repository. 
Usually you will also see the changes you are about to commit.
The *commit message* is something really important and the message should be meaningful and readable as it will be later used by others to understand your action. 
There are several ways to do this and it boils down to what you team wants but here are some links on good commit messages:
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [https://cbea.ms/git-commit/](https://cbea.ms/git-commit/)

The same can be achieved directly in the Terminal by writing
```bash
> git commit -m "feat: add my solution for python exercises 1"
[main 578e48f] feat: add my solution for python exercises 1
 2 files changed, 90 insertions(+)
 create mode 100644 python_ex1/ID.py

```
\figenv{Commiting changes the repository}{/assets/pages/git/commit.svg}{}

@@important
Any changes done to a file after running `git add <file>` will not be part of a _commit_. If they should be included you need to rerun `git add <file>`.
@@

@@important
My submitting an _empty_ commit message you can about a commit.
@@

Now the changes are in the _local repository_ and you can continue working. 
In order to share your changes with others you need to get them to the _remote repository_. 
This is done by _pushing_ the changes. 

```bash
> git push
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 16 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 1.31 KiB | 1.31 MiB/s, done.
Total 5 (delta 0), reused 0 (delta 0), pack-reused 0
To https://git.uibk.ac.at/c102338/ulg22_playground.git
   e8f13f4..578e48f  main -> main

```
\figenv{Pushing changes from the local to the remote repository}{/assets/pages/git/push.svg}{}
