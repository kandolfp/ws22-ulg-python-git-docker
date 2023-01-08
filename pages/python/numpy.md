@def title = "python - numpy"
@def hascode = true

@def tags = ["python, numpy"]


# Introduction to NumPy

[NumPy](https://numpy.org/) is a Python package for scientific computing with an open source licence ([BSD license](https://github.com/numpy/numpy/blob/main/LICENSE.txt)). 
It provides a _multidemsional array_ object and a large collection on high-level mathematical routines for fast operations on these arrays. 
This includes logical operations, shape manipulation, sorting, selecting, basic linear algebra (BLAS), basic statistical operations, random simulation and much more. 
It is wildly used and the basis for many other packages.
It was introduced to allow **fast** array computation in python and it is keeping its promises ever since. 

NumPy itself is build on other components such as [Basic Linear Algebra Subroutines](https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms) written in Fortran, C, CUDA or whatever required for the specific task. 
For this purpose it can be linked against Intel Math Kernel Library or OpenBLAS.   

These notes are heavily inspired by the notes of Gregor Ehresperger from previous years in this class.  
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

We start of with one dimensional arrays:
```python
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
and extend to two dimensional arrays as:
```python
>>> Y = np.array([[1.1, 2.1, 3.1], [1.2, 2.2, 3.2]])
# element at row and column 0
>>> Y[0, 0]
1.1

# element at row 0 and column 2
>>> Y[0, 2]
3.1

# element at row 1 and second to last column
>>> X[1, -2]
2.2
```
Of course we can also use [slicing](https://en.wikipedia.org/wiki/Array_slicing), i.e. accessing subarrays, with the following rules (per dimension)
`x[start:stop:step]`
- _start_ is by default 0
- _stop_ is by default the length of the dimension
- _step_ is by default 1
This allows us to do do the following.
```python
>>> X = np.array([1, 2, 3, 5, 7])

>>> X[0:3:1]
array([1, 2, 3])

>>> X[:3]
array([1, 2, 3])

>>> X[3:]
array([5, 7])

>>> X[::2]
array([1, 3, 7])

>>> Y = np.array([[1.1, 2.1, 3.1], [1.2, 2.2, 3.2]])
# first row
>>> Y[0, :]
array([1.1, 2.1, 3.1])

# last row
>>> Y[-1, :]
array([1.2, 2.2, 3.2])

# every second column
>>> Y[:, ::2]
array([[1.1, 3.1],
       [1.2, 3.2]])
```

\exercise{
Please solve the following exercises:
1. Create the following matrix as an _numpy array_
$$
 A = \begin{pmatrix}
     0 &  1 &  2 &  3 &  4\\
     5 &  6 &  7 &  8 &  9\\
    10 & 11 & 12 & 13 & 14\\
    15 & 16 & 17 & 18 & 19\\
    \end{pmatrix}.
$$
**Hint** look into the NumPy functions `arange` and `reshape`.
1. How do you access the element `7`?
1. How do you access the entire third column?
1. Return an array with only the odd rows and even columns:

\solution{
```python
>>> A  = np.reshape(np.arange(0, 20), (4, 5))
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14],
       [15, 16, 17, 18, 19]])

>>> A[1,2]
7

>>> A[:, 2]
array([ 2,  7, 12, 17])

>>> A[1::2, ::2]
array([[ 5,  7,  9],
       [15, 17, 19]])
```
}
$\,$}

# Link to the session notes
[Session Notes](/assets/pages/python/Session1.html)
