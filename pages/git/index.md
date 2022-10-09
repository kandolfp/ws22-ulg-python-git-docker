@def title = "Git"
@def hascode = true

@def tags = ["git"]

# What is a version control software

A Version Control Software (VCS) is, in its most abstract form, designed to keep track of changes to a file system. 
This means it tracks changes in files, such as addition or deletion, but also more basic modifications to files or directories. 
A VCS also allows teams to collaborate and therefore inform each other of changes made by others. 
Some of the main features of a VCS is that you can: 
1. Revert a file or an entire directory to a previous state.
1. Compare files or directories to a previous version.
1. Help resolving conflicts if multiple changes on the same resource collide with each other. 
1. Allows you to _tag_ specific versions of a file for alter use.
1. Keep track who changed what and when.
1. Much more

In the scope of an entire file system this is used by operation systems to minimize file loss and allows users to restore unwanted changes. 
Most often, and that is also the main focus here, a VCS is used to track of source code. 
This might be a software project or a term paper. 
Nevertheless, the VCS will  keep track of additions, deletions, modifications and so forth of individual lines of _text_ within these files. 
We will focus on *Git* but there are alternatives around such as _Mercurial_, _CVS_, or _SVN_.
All of them have individual strengths and weaknesses but in terms of wide spread use Git is in the lead. 

# What is git and a bit of history

Git was developed as a free and open source software by [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) in 2005 for the development of the Linux kernel. 
The main goals in the development were (according to [wikipedia](https://en.wikipedia.org/wiki/Git)):
- Take Concurrent Versions System (CVS) as an example of what not to do; if in doubt, make the exact opposite decision.
- Support a distributed, [BitKeeper](https://en.wikipedia.org/wiki/BitKeeper)-like workflow.
- Include very strong safeguards against corruption, either accidental or malicious.

> The development of Git began on 3 April 2005. Torvalds announced the project on 6 April and became self-hosting the next day. The first merge of multiple branches took place on 18 April. Torvalds achieved his performance goals; on 29 April, the nascent Git was benchmarked recording patches to the Linux kernel tree at the rate of 6.7 patches per second. On 16 June, Git managed the kernel 2.6.12 release.

Regarding the name:
> Torvalds sarcastically quipped about the name git (which means "unpleasant person" in British English slang): "I'm an egotistical bastard, and I name all my projects after myself. First 'Linux', now 'git'." The man page describes Git as "the stupid content tracker". The read-me file of the source code elaborates further:

"git" can mean anything, depending on your mood.

- Random three-letter combination that is pronounceable, and not actually used by any common UNIX command. The fact that it is a mispronunciation of "get" may or may not be relevant.
- Stupid. Contemptible and despicable. Simple. Take your pick from the dictionary of slang.
"Global information tracker": you're in a good mood, and it actually works for you. Angels sing, and a light suddenly fills the room.
- "Goddamn idiotic truckload of sh&ast;t": when it breaks.

In the following we try to give a pragmatic hands on introduction to the concepts of git and how it can be uses. 
The notes are a mix of several sources but the main ideas are based on the [git training by the UnseenWizzard](https://github.com/UnseenWizzard/git_training).


This part covers the following sections:

1. [Installation and basics](install/)



# Additional resources
Obviously not everybody learns the same way and the concepts of Git can make your brain twist a bit so here are some additional resources:
- [Git cheat sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Pro Git](https://git-scm.com/book/en/v2) a book on git
- [A Visual Git Reference](https://marklodato.github.io/visual-git-guide/index-en.html)