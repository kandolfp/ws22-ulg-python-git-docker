@def title = "Git - Basics"
@def hascode = true

@def tags = ["git, basics"]


# Basics of Git
Git is a distributed VCS, in this case this means the entire _repository_ is distributed on various machines and possible multiple _remotes_. 
This was a clear design choice that allows individual contributors to work independently of the availability of a _remote_ but still have the full history available. 
The following is based on the [git training by the UnseenWizzard](https://github.com/UnseenWizzard/git_training). 
The main structure as well as the basic idea in the pictures follows this notes, with a view adaptations where needed. 

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

This has we modified our _working directory_. In order to get an idea what Git thinks about this lets run `git status` in the working directory:
```git
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	python_ex1/ID.py

nothing added to commit but untracked files present (use "git add" to track)
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

By rerunning `git status` we get
```git
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
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
 1 files changed, 85git insertions(+)
 create mode 100644 python_ex1/ID.py

```
\figenv{Commiting changes the repository}{/assets/pages/git/commit.svg}{}

In the above message you can see that your commit get some more meta data. 
Specifically, it gets a [SHA-1](https://de.wikipedia.org/wiki/Secure_Hash_Algorithm) hash, namely `578e48f`
The hash is used to keep track of your commits and is one of the ground breaking ideas that makes Git so successful. 
The hash is much longer but due to its nature in most cases already its first seven digits will make it unique. 

@@important
Any changes done to a file after running `git add <file>` will not be part of a _commit_. If they should be included you need to rerun `git add <file>`.
@@

@@important
My submitting an _empty_ commit message you can about a commit.
@@

Now the changes are in the _local repository_ and you can continue working. 
In order to share your changes with others you need to get them to the _remote repository_. 
This is done by _pushing_ the changes. 
We do this by calling `git push` witch gives us an output similar to:
```git
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 16 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.17 KiB | 1.17 MiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
To https://git.uibk.ac.at/c102338/ulg22_playground.git
   e8f13f4..0b8a494  main -> main
```
\figenv{Pushing changes from the local to the remote repository}{/assets/pages/git/push.svg}{}

Now, if you would do this on your own everything is working and your are happy. 
Unfortunately, as we are doing this in a class and all at the same time we will run into some difficulties. 
After all, this is a crash course for Git, eventually something hat to crash. 

Some of you might get the following message for `git push`:
```git
To https://git.uibk.ac.at/c102338/ulg22_playground.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://git.uibk.ac.at/c102338/ulg22_playground.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This gives us the opportunity to talk about how to get changes from the _remote repository_ to your _local repository_ after the initial _clone_. 
Unfortunately, in order to do this properly we need to talk about branches first. 

# Branches

The word _branch_ was mentioned several times before but not explained. 
The main idea is rather simple. 

If you consider having one commit after the other in a long chain like the trunk of a tree a branch is the same as for a tree:
\figenv{A branch in a chain of commits}{/assets/pages/git/branching.svg}{}
In short, whenever multiple commits are _based_ on the same commit they (and all following commits) form different branches.

By default, git always operates on _branches_. 
When we cloned the _remote repository_ we also cloned its branches and we started working on the `main` branch. 
You can go back and check the messages, it is always there. 

Now without knowing we created a branch. 
It is not visible to us but it is clear from the point of the _remote repository_. 

There are several ways of _integrating_ or _merging_ two branches back into one.

For now we will only talk about the most elegant and simplest way, with a `rebase`. 

Naturally, every branch is _based_ on a commit. 
In the above example `9a98eb2` is based on `e8f13f4`. 
As the name suggests, _rebase_ simple changes this base. 
This gives us a clean way of how the entire Git commit chain is supposed to be read.
One way of performing a rebase we will see shortly. 
But first, lets talk about how to 

# Integrating remote changes into your local environment

The above _error_ message gives us already a hint on what to do but lets make it more structured. 

By running a 
```git
> git status
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```
we can see that the _remote_ and the _local_ repository have different commits. 

With `git fetch` you can get changes from the _remote repository_ into the _local repository_. This is the other way around as with the `git push` command.
\figenv{Fetching changes from the remote to the local repository}{/assets/pages/git/fetch.svg}{}

The important part here is, that this does not effect your _working directory_ as the changes are only synchronized with the _local repository_ and when you try to push again you will see the same message. 
It does not even effect your _local branches_, it will only make sure that all of the _remote branches_ are synchronized. 


## Pulling

In order to affect the _working directory_ and your _local branches_, we need to pull the changes in. 
This is done with `git pull`. 

As we have some _conflicts_ we need to define a strategy how to deal with them. 
At the moment we only know one so let us use
```bash
> git pull --rebase
Successfully rebased and updated refs/heads/main.
```
This should have worked for everybody as all of you added different files to the repository and the Git tree looks something like this:
\figenv{After pulling and rebasing}{/assets/pages/git/simplerebase.svg}{}

Next we will see what happens if we modify some files. 

# Modifying content in a repository

A good start to do this is to link our uploaded file form the `README` contained in the repository. 

With your favourite editor add the following content next to your `ID` (btw. this is [markdown](https://en.wikipedia.org/wiki/Markdown) syntax):
```markdown
# ulg22_playground

## List of handin the python exercises 

| Name/UID    | File        |
| ----------- | ----------- |
| ID     | [my upload](python_ex1/ID.py) |
```

If we check with `git status` we can see that `README.md` is modified
```git
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

```

Of course this is only a change in our _working directory_ and not in either of the two _repositories_. 
Before we add the changes to the _local repository_ we can use `git diff` to see what we actually changed. 

```diff
diff --git a/README.md b/README.md
index d9f0acb..9ac3846 100644
--- a/README.md
+++ b/README.md
@@ -4,7 +4,7 @@
 
 | Name/UID    | File        |
 | ----------- | ----------- |
-| ID  | |
+| ID  | [my upload](python_ex1/ID.py)|
 | ID1 | |
 | ID2 | |
 | ID3 | |
```

We already know the next steps, _add_, _commit_, and _push_.

So lets recall, with `git add README.md` we move the file into the staging area. 
**Note:** If you run `git diff` now, the output is empty. This is because, `git diff` only works on the changes in your _working directory_. You can still get the diffs from your _staging area_ with `git diff --staged` (some editors will use this if you type up your _commit message_).

Now, before we commit, we decide to modify `README.md` again. Maybe we made a typo or we just really want to nail this hand in so we change it, maybe we add
```markdown
| ID     | [my upload](python_ex1/ID.py) run it by calling `python3 python_ex1/ID.py`|
```
to make it clear we know what we are doing. 

If we run `git status` we see the following
```git
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

```
which tells us that `README.md`is modified and staged. 

If we run a `git diff` again we get
```diff
diff --git a/README.md b/README.md
index 9ac3846..28dba4b 100644
--- a/README.md
+++ b/README.md
@@ -4,7 +4,7 @@
 
 | Name/UID    | File        |
 | ----------- | ----------- |
-| ID  | [my upload](python_ex1/ID.py)|
+| ID  | [my upload](python_ex1/ID.py) run it by calling `python3 python_ex1/ID.py`|
 | csad3581 | |
 | csak1512 | |
 | csak4299 | |
```
which show the changes to the _staging area_. 
If we are satisfied with our changes, we can use `git add README.md` again to add the file to the _staging area_ and finally commit it with `git commit`.
Of course we do this **with a meaningful commit message**.

Depending on your timing, you might have to _fetch_ and _pull_ in changes to your _local repository_. 
By the way, you can directly call `git pull`, without first calling `git fetch`, the fetch is done implicitly.  