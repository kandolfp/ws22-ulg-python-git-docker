@def title = "ULG 971001 - Software (VU 2) - python, git, docker"
@def tags = ["syntax", "code"]

# 971001 Software - Introduction to python, git and docker
~~~
<p class="author"><em>Peter Kandolf <a href='https://orcid.org/0000-0003-3601-0852' class='ORCID'></a></em></p>
<p class="date"><em>Last updated: {{ fill fd_mtime }}</em></p>
~~~

# Preface

This notes are under construction!
 

# Getting started 


## Acknowledgment

We want to thank the open source community for providing excellent tutorial and guides for Python, Git, and Docker on the web. Individual sources are cited at the appropriate spot. 
We also want to thank Elias Tappeiner from the Department of _Biomedizinische Bildanalyse
UMIT, Hall in Tirol, Österreich_ for his slides on Git.


## Some general words on the organization of this workshop

For this workshop you do not need to be an Julia expert, not even a programming expert. We will introduce programming and Julia concepts along with examples. For this purpose you will find code blocks that you can copy if necessary (top right corner `copy`). Furthermore, if we think something is very important we will highlight it with a box:

@@important
Pay extra attention to the content here.
@@

We also have various environments with different color coding that usually are collapsed to not hinder the reading process:
- Examples are green
\example{Examples are also quite nice because they make sure you have an idea what is happening.}
- Exercises are blue
\exercise{Exercises are here for you to work on, no worries not at home or in your off hours but with us in the workshop.

Some exercises will have *hidden* solutions which can be accessed by adding `?solution=true` to the URL of the page. 
This way it is more likely that everybody tries to do the exercises without first looking at the sample solution.}

That is about all we wanted to let you know, so let us get into it.
