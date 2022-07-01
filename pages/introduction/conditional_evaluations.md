@def title = "Conditional evaluations"
@def hascode = true

@def tags = ["conditional_evaluations"]

# Conditional evaluations
In the following, we start discussing [control flows](https://docs.julialang.org/en/v1/manual/control-flow/#Control-Flow). Let us start with conditional statements.
Recall that Julia provides boolians, that is, variables that can be either `true` or `false`. Often, an algorithm wants to react to a statement being `true` or `false`. As an example, we would like to check if a given input is correct and if it is not print a warning message. This sentence already shows that we need an `if` statement, which takes the form
```julia
if condition
    # code to be run if condition is true
end
```
As an example, let us choose a number and check if this number is stored as a Float of an Int
```julia
number  = 2
if typeof(number) == Int
    println("This number is an integer")
end
```
Now if this number is not an integer, we would like to print out this information. This can be done with the `else` statement
```julia
number  = 2.0
if typeof(number) == Int
    println("This number is an integer")
else
    println("This number is not an integer")
end
```
Moreover, if we want to check whether the number is a float in case it is not an integer we can use the `ifelse` statement
```julia
number  = 2.0
if typeof(number) == Int
    println("This number is an integer")
elseif typeof(number) == Float64
    println("This number is a float")
else
    println("This number is neither a float nor an integer")
end
```
Note that in comparison to other programming languages, `if` blocks can define new variables which can be used outside of these blocks.
```julia
number  = 2.0
if typeof(number) == Int
    info = "This number is an integer"
else
    info = "This number is not an integer"
end
println(info)
```