# ULG Data Science - WS22 971001 Software - Introduction to python, git, and docker

Course material for a ~4+4 hours introduction to git (with CI/CD and gitlab), docker and a hint of python.

# Citing this software
[Citation information](CITATION.cff)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10532727.svg)](https://doi.org/10.5281/zenodo.10532727)

# Development
We use [Franklin.jl](https://franklinjl.org) to generate the lecture material. To do so, simply activate the environment, use Franklin and run the local server:
```
activate .
using Franklin
serve()
```
or run `julia start.jl` for the main directory.

To update the material edit the `md` files in `pages/`. The main landing page can be found in `index.md`.

To add a new subpage, create a new `md` file and save it at the appropriate place within the subdirectory structure `pages/`. Additionally the file needs to be added to the navigation bar which can be achieved by updating `_layout/pgwrap.html`.

Have a look at the documentation of [Franklin.jl](https://franklinjl.org) for more information (use the search box to easily find what you are looking for). Additionally you may consult the [demos](https://franklinjl.org/demos/) page.

# Publishing
After pushing the published website will automatically be built and deployed at [kandolfp.github.io/ws22-ulg-python-git-docker](https://kandolfp.github.io/ws22-ulg-python-git-docker/) (this might need a couple of minutes).