@def title = "ULG 971001 - Software (VU 2) - python, git, docker"
@def tags = ["syntax", "code"]

# 971001 Software - Introduction to python, git and docker
~~~
<p class="author"><em>Peter Kandolf <a href='https://orcid.org/0000-0003-3601-0852' class='ORCID'></a></em></p>
<p class="date"><em>Last updated: {{ fill fd_mtime }}</em></p>
~~~

# Preface

Course material for a ~4+4 hours introduction to git (with CI/CD and gitlab), docker and a hint of python.

## Citation
In case you want to refer to this lecture material, use the following BibTex snippet:
```Bibtex
@misc{pgd2024,
    doi = {10.5281/zenodo.10532424},
    url = {https://kandolfp.github.io/ws22-ulg-python-git-docker/},
    author = {Kandolf, Peter},
    keywords = {FOS: Mathematics},
    language = {en},
    title = {ULG Data Science - WS22 971001 Software - Introduction to python, git, and docker},
    year = {2024},
    copyright = {Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International}
}
```
or use [CITATION.cff](CITATION.cff).

# Getting started 


## Acknowledgment

We want to thank the open source community for providing excellent tutorial and guides for Python, Git, and Docker on the web. Individual sources are cited at the appropriate spot. 
We also want to thank Elias Tappeiner from the Department of _Biomedizinische Bildanalyse
UMIT, Hall in Tirol, Österreich_ for his slides on Git and
Mirjam Ziselsberger for trying, checking, suggestions and additional material in the CI/CD section.


## Some general words on the organization of this workshop

This material is intended to aid during class and give you some extra material that might be to fast in class and you want to read over in your own time again. 

It is split up into the three parts for *Python*, *Git*, and *Docker*. 
Where sometimes we will jump between the sections during lecture we try to write it down in such a way that they are self contained. 
In general, you will find code blocks that you can copy if necessary (top right corner `copy`). Furthermore, if we think something is very important we will highlight it with a box:

@@important
Pay extra attention to the content here.
@@

We also have various environments with different color coding that usually are collapsed to not hinder the reading process:
- Examples are green
\example{Examples are also quite nice because they make sure you have an idea what is happening.}
- Exercises are blue
\exercise{Exercises are here for you to work on, no worries not at home or in your off hours but with us in the workshop.
}

That is about all we wanted to let you know, so let us get into it.
