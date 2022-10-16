@def title = "python - numpy"
@def hascode = true

@def tags = ["python, numpy"]


# Introduction to Numpy

[Numpy](https://numpy.org/) is a Python package for scientific computing with an open source licence ([BSD license](https://github.com/numpy/numpy/blob/main/LICENSE.txt)). 
It provides a _multidemsional array_ object and a large collection on high-level mathematical routines for fast operations on these arrays. 
This includes logical operations, shape manipulation, sorting, selecting, basic linear algebra (BLAS), basic statistical operations, random simulation and much more. 
It is wildly used and the basis for many other packages.
It was introduced to allow **fast** array computation in python and it is keeping its promises ever since. 

Numpy itself is build on other components such as [Basic Linear Algebra Subroutines](https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms) written in Fortran, C, CUDA or whatever required for the specific task. 
For this purpose it can be linked against Intel Math Kernel Library or OpenBLAS.   

# First steps

We load the package as per usual and by common convention we give it the name`np`. 

```python
import numpy as np
```

We can create arrays with a multitude of function
```python
# 1D-array with 5 integers
X = np.array([1, 2, 3, 5, 7])

# 2x3 2D-array with floating point numbers
Y = np.array([[1.1, 2.1, 3.1], [1.2, 2.2, 3.2]])

# We can also create an array of zeros
Z = np.zeros((2, 3))

# Or an equally spaced array of 6 elements between 1 and 10
A = np.linspace(1, 10, 6)
```
Now that we have our arrays we also need to talk about how to access the elements. 
@@important
Python uses 0 based indexing!
@@

```python-repl
>>> X = np.array([1, 2, 3, 5, 7])
>>> X[2]
3
>>> X[0]
1
>>> X[-1]
7
>>> X[6]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: index 6 is out of bounds for axis 0 with size 5
```