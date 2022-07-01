@def title = "Parallel Computing - Multithreading"
@def hascode = true

@def tags = ["ParallelComputingMultithreading"]

# Multithreading in Julia

Before we have a look how [Julia deals with the concept of multithreading](https://docs.julialang.org/en/v1/manual/multi-threading/), let us make clear what we are talking about.

## What is multithreading

In the terminology of computer science a thread is the smallest sequence of instructions that can be managed by the scheduler of the operating system. It is often also called a light weight process and is most of the time considered to exist inside the context of a process. Consequently, multithreading is the ability to mange multiple concurrently executed threads. Multiple threads share their resources, this makes this quite a powerful tool. The threads run on a single CPU or on multiple CPUs and give you the opportunity to leverage the full force of your computer (or cell phone for that matter).

## Julia

By default, Julia will start with a single computational thread of execution:
```julia-repl
julia> Threads.nthreads()
1
```
This does not mean that Julia is only using one thread. We mentioned the [Basic Linear Algebra Subroutines - BLAS](http://www.netlib.org/blas/) before. Calls to this library (e.g. matrix-matrix multiplication) will be multithreaded by default and therefore, technically you have been doing multithreading all along ;). Let us illustrate this with a small example.

\example{
Influencing the number of threads in BLAS. When we include the [LinearAlgebra](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/) package we get the possibility to manipulate the number of threads. Here `BLAS` is a wrapper to the BLAS libraries used by Julia. 
```julia-repl
julia> using BenchmarkTools
julia> using LinearAlgebra
julia> A = rand(2000, 2000);
julia> B = rand(2000, 2000);
julia> @btime $A*$B;
  141.984 ms (2 allocations: 30.52 MiB)
julia> BLAS.get_num_threads()
8
julia> BLAS.set_num_threads(1)
julia> @btime $A*$B;
  1.009 s (2 allocations: 30.52 MiB)
```
}

In order to have multiple threads available you need to start Julia with the `--threads <Int|auto>` option or define the environment variable `JULIA_NUM_THREADS=<Int>`. With the command `threadid()` from the [`Threads`](https://docs.julialang.org/en/v1/base/base/#Base.Threads) module you can find out on which thread you currently are.
```julia-repl
> julia --threads auto
julia> Threads.nthreads()
16
juliy> Threads.threadid()
1
```

We do not have the time for a deep dive into all the dirty details on how do do proper multithreaded programming (raise conditions, locks, atomic operations, thread safe programming, ...), therefore we keep it light and simple with the `@threads` macro and introduce the needed concepts when we need them along the way.

Like all the other macro it gives us the possibility to bring something rather complex in our code by still staying very readable as the following example shows.

\example{
Simple example for the [docs](https://docs.julialang.org/en/v1/manual/multi-threading/#The-@threads-Macro):

```julia-repl
julia> using Base.Threads
julia> nthreads()
4
julia> a = zeros(10);
julia> @threads for i in 1:10
                    a[i] = threadid()
                end
julia> a
10-element Vector{Float64}:
 1.0
 1.0
 1.0
 2.0
 2.0
 2.0
 3.0
 3.0
 4.0
 4.0
 ```
}

Let us try to apply this example to our $\pi$ example.

## Multithreaded $\pi$

The obvious first impulse is to just write a `@threads` in front of the loop in our `in_unit_circle` routine, well lets follow this impulse.

@@important
We assume the functions and variables from the [Not the most efficient way of computing $\pi$](./pi) section are defined and we write some new in addition.
@@

For reference, the results in this section are computed with 4 threads and the original code has the following performance
```julia-repl
julia> @btime estimate_pi(in_unit_circle, N)
  235.643 ms (0 allocations: 0 bytes)
3.14141152
```
\exercise{
Define a new function `in_unit_circle_threaded1` with the `@threads` macro and test the result as well as the timing.
\solution{
```julia
using BenchmarkTools

function in_unit_circle_threaded1(N::Int64)
    M = 0
    
    @threads for i in 1:N
        if (rand()^2 + rand()^2) < 1
            M += 1
        end
    end

    return M
end
```
and we test it
```julia-repl
julia> println(
           abs(
           estimate_pi(in_unit_circle_threaded1, N) - pi
           )
       )
2.238629413589793

julia> @btime estimate_pi(in_unit_circle_threaded1, N)
  1.764 s (78540628 allocations: 1.17 GiB)
0.8047274
```
Well that is underwhelming. The result is wrong and it is slower. So what happened?
}
}

### Atomic Operations

As we could see in the above example of the docs the loop is automatically split up per index for the threads available. This means each of the threads is performing the same loop and as the context and memory is shared also access the same storage. This is problematic for our variable `M`. This means each thread reads and writes in the same variable but this also means the result is not correct. It might override the results of other threads or they all read at the same time but only one result will be written in the end. In short the counter is totally wrong. We call this [rase condition](https://en.wikipedia.org/wiki/Race_condition).

To solve this issue Julia supports [atomic](https://docs.julialang.org/en/v1/manual/multi-threading/#Atomic-Operations) values. This allows us to access a variable in a thread-safe way and avoid race conditions. All primitive types can be wrapped with `M = Atomic{Int}(0)` and can only be accessed in a thread safe way. In order to do the atomic add we use the function `atomic_add!(M, 1)` and we can access the value with `M[]`.

\exercise{
Define a new function `in_unit_circle_threaded2` with the `@threads` macro, an atomic `M` and test the result as well as the timing.
\solution{
```julia
function in_unit_circle_threaded2(N::Int64)
    M = Atomic{Int}(0);
    
    @threads for i in 1:N
        if (rand()^2 + rand()^2) < 1
            atomic_add!(M, 1)
        end
    end

    return M[]
end
```
and we test it
```julia-repl
julia> println(
           abs(
           estimate_pi(in_unit_circle_threaded2, N) - pi
           )
       )
1.1466410207106037e-5

julia> @btime estimate_pi(in_unit_circle_threaded2, N)
  1.026 s (78540628 allocations: 1.17 GiB)
3.14157478
```
Now our result is correct, but the time is still not better but worse.
}
}

### Actually distribute the work
We are still not fast because each `attomic_add!` is checking which thread has the current result and needs to add the new value. To avoid this we need to eliminate atomic again. We can actually split up the work quite neatly if we remember the example from the docs. It is possible to access the `threadid()` and the number of threads `nthreads()`. So why not define `M` as an array of length `nthreads()` and in each thread write to separate values in the array by using `threadid()` as index.

\exercise{
Define a new function `in_unit_circle_threaded3` with the `@threads`, `M` as array and test the result as well as the timing.
\solution{
```julia
function in_unit_circle_threaded3(N::Int64)
    M = zeros(Int64, nthreads());
    
    @threads for i in 1:N
        if (rand()^2 + rand()^2) < 1
            @inbounds M[threadid()] += 1
        end
    end

    return sum(M)
end
```
and we test it
```julia-repl
julia> println(
           abs(
           estimate_pi(in_unit_circle_threaded2, N) - pi
           )
       )
0.00017913358979315674in

julia> @btime estimate_pi(in_unit_circle_threaded2, N)
  246.722 ms (22 allocations: 2.05 KiB)
3.1415168
```
Now our result is correct, and the time is okay.
}
}

Now we are faster, but still not faster than the serial version. Why is it still not working?

### Global states

Without going into too much detail, `rand()` is not thread safe. It does manipulate and read from some global state and that causes our slowdown. In fact, as the random numbers are not correctly distributed any more the accuracy is also decaying. 

To avoid this we need to exchange the random number generator and make the call to `rand` thread safe. This solution is inspired by the section *Multithreading* of the [Parallel Computing Class on JuliaAcademy.com](https://juliaacademy.com/p/parallel-computing) and slightly adapted for the setup we have. 

First step is to define a separate random number generator per thread:
```julia
import Random

const ThreadRNG = Vector{Random.MersenneTwister}(undef, nthreads())
@threads for i in 1:nthreads()
       ThreadRNG[threadid()] = Random.MersenneTwister()
end
```
What we do in the third line is define a [`const`](https://docs.julialang.org/en/v1/base/base/#const) variable. That is a global variable whose value will not change. In fact we define a Vector of size `nthreads()` and fill it with distinct [`Random.MersenneTwister`](https://docs.julialang.org/en/v1/stdlib/Random/#Random.MersenneTwister). This allows us to have a different random number generator for each thread by using
```julia
rng = ThreadRNG[threadid()]
rand(rng)
```
in each thread. 

Now for our final version of the code, the basic idea is to not have a threaded loop over the integer `N` but over the number of threads. In order for this to work we need to figure out how many iterations each threads needs and for that we use `len, rem = divrem(N, nthreads())` to divide up `N` into the quotient and remainder from the Euclidean division.  

\exercise{
Define a new function `in_unit_circle_threaded4` with the `@threads` macro, `M` as array, the above code snippets and test the result as well as the timing.
Extra points if you check if we do not loose any iterations due to the split.
\solution{
```julia
import Random

const ThreadRNG = Vector{Random.MersenneTwister}(undef, nthreads())
@threads for i in 1:nthreads()
       ThreadRNG[threadid()] = Random.MersenneTwister()
end

function in_unit_circle_threaded4(N::Int64)
    M = zeros(Int64, nthreads())
    len, rem = divrem(N, nthreads())
    
    @threads for i in 1:nthreads()
        rng = ThreadRNG[threadid()]
        m = 0
        for j in 1:len
            if (rand(rng)^2 + rand(rng)^2) < 1
                m += 1
            end
        end
        M[threadid()] = m
    end

    return sum(M)
end 
```
and we test it
```julia-repl
julia> println(
           abs(
           estimate_pi(in_unit_circle_threaded4, N) - pi
           )
       )
3.797358979307219e-5

julia> @btime estimate_pi(in_unit_circle_threaded4, N)
  ???.??? ms (22 allocations: 2.05 KiB) -> do with same laptop
3.141406
```
}
}

### Be aware of oversubscription

We should mention that you can overdo it with threading. For example if multithread something that uses a BLAS routine. Now inside each thread something is trying to run on multiple threads. As a result, they might get into the way of each other and the overall performance is reduced.  
