@def title = "python - pandas"
@def hascode = true

@def tags = ["python, pandas"]

# Introduction to Pandas

[Pandas](https://pandas.pydata.org/) is a Python package for data analysis and manipulation tool with an open source licence ([BSD license](https://github.com/pandas-dev/pandas/blob/main/LICENSE)). 
It provides a data structure called _DataFrame_ - essentially a multidimensional array with row and column labels - that is high-performant, easy-to-use and flexible.
Pandas is build on top of NumPy, so it gains all the optimisations and functions that are associated with this package. 

These notes are heavily inspired by the notes of Gregor Ehrensperger from previous years in this class.  

# First steps

We load the package as per usual and by common convention we give it the name`pd`. 

```python
import pandas as pd
```
A Pandas Series is a 1D array of indexed data

```python
>>> data = pd.Series([0.25, 0.5, 0.75, 1.0])
# show the entire Series
>>> data
0    0.25
1    0.50
2    0.75
3    1.00
dtype: float64

# just show the values of the series
>>> data.values
array([0.25, 0.5 , 0.75, 1.  ])

# just show the index of the series
>>> data.index
RangeIndex(start=0, stop=4, step=1)

# accessing elements works just like in numpy
>>> data[2]
0.75

# slicing is also available
>>> data[1:3]
1    0.50
2    0.75
dtype: float64
```

So far we are essentially using NumPy functionality, the essential difference is that the Pandas Series (implicitly) defines an index.
```python
# explicitly define an index
>>> data = pd. Series ([0.25 , 0.5 , 0.75 , 1.0] ,
                  index =["a",  "b",   "c", "d"])

# have a look
>>> data
a    0.25
b    0.50
c    0.75
d    1.00
dtype: float64

# display the index used
>>> data.index
Index(['a', 'b', 'c', 'd'], dtype='object')

# you can still use the _implicit_ numerical index
>>> data[1:3]
b    0.50
c    0.75
dtype: float64

# or directly the new index
>>> data["b"]
0.5

# with all the variants available
>>> data[["b", "c"]]
b    0.50
c    0.75
dtype: float64
```

Of course we can also use a dictionary as the start of our series

```python
# define the dictionary with first and last name
>>> users_dict = {"Alice ": "Lidell ",
                  "Bob": "Ross",
                  "Charlie": "Chaplin"}

# create a Pandas Series out of the dictionary
>>> data = pd.Series(users_dict)

# display the series
>>> data
Alice      Lidell 
Bob           Ross
Charlie    Chaplin
dtype: object

# show the indeces
>>> data.index
Index(['Alice ', 'Bob', 'Charlie'], dtype='object')

# show the data values
>>> data.values
array(['Lidell ', 'Ross', 'Chaplin'], dtype=object)

# have a look what pandas tells us about the series
>>> data.describe()
count           3
unique          3
top       Lidell 
freq            1
dtype: object

```

If you go to the already mentioned `DataFrame` you get a 2D array with flexible row and column names/indices.

First we define two series with the area and the population of the states in Austria
```python
>>> area_dict = {"Vienna": 415,   "Lower Austria": 19178,
                 "Styria": 16401, "Upper Austria": 11982,
                 "Tyrol": 12648,  "Carinthia": 9536,
                 "Salzburg": 7154,"Vorarlberg": 2601,
                 "Burgenland": 3965}
>>> pop_dict = {"Vienna": 1794770, "Lower Austria": 1636287,
                "Styria": 1221014, "Upper Austria": 1436791,
                "Tyrol": 728537,   "Carinthia": 557371,
                "Salzburg": 538258,"Vorarlberg": 378490,
                "Burgenland": 288229}
>>> area = pd.Series(area_dict)
>>> pop = pd.Series(pop_dict)
```

and combine them to a `DataFrame`
```python
>>> states = pd.DataFrame({"area": area, "population": pop})

>>> states
                area  population
Vienna           415     1794770
Lower Austria  19178     1636287
Styria         16401     1221014
Upper Austria  11982     1436791
Tyrol          12648      728537
Carinthia       9536      557371
Salzburg        7154      538258
Vorarlberg      2601      378490
Burgenland      3965      288229


>>> states.loc[["Vienna", "Lower Austria"], "population"]
Vienna           1794770
Lower Austria    1636287
Name: population, dtype: int64
```

Of course you can have higher functions working on the `DataFrame`
```python
# Show us some standard statistics 
>>> states.describe()
               area    population
count      9.000000  9.000000e+00
mean    9320.000000  9.533052e+05
std     6357.483543  5.736115e+05
min      415.000000  2.882290e+05
25%     3965.000000  5.382580e+05
50%     9536.000000  7.285370e+05
75%    12648.000000  1.436791e+06
max    19178.000000  1.794770e+06

#which states have less than one million inhabitants?
>>> states["population"] < 1e6
Vienna           False
Lower Austria    False
Styria           False
Upper Austria    False
Tyrol             True
Carinthia         True
Salzburg          True
Vorarlberg        True
Burgenland        True
Name: population, dtype: bool
```

You can also add new or derived values to the `DataFrame`
```python
# get the population density
>>> states["density"] = states["population"]/states["area"]
>>> states
                area  population      density
Vienna           415     1794770  4324.746988
Lower Austria  19178     1636287    85.321045
Styria         16401     1221014    74.447534
Upper Austria  11982     1436791   119.912452
Tyrol          12648      728537    57.600965
Carinthia       9536      557371    58.449140
Salzburg        7154      538258    75.238748
Vorarlberg      2601      378490   145.517109
Burgenland      3965      288229    72.693317
```

You can also sort according to a column
```python
>>> states.sort_values(by=["density"])
                area  population      density
Tyrol          12648      728537    57.600965
Carinthia       9536      557371    58.449140
Burgenland      3965      288229    72.693317
Styria         16401     1221014    74.447534
Salzburg        7154      538258    75.238748
Lower Austria  19178     1636287    85.321045
Upper Austria  11982     1436791   119.912452
Vorarlberg      2601      378490   145.517109
Vienna           415     1794770  4324.746988
```

# Link to the session notes
[Session Notes](/notebooks/html/Session2/)
