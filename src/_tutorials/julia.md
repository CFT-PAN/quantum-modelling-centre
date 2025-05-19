---
title: "Julia"
---
# Installation
To install julia, you should download and install the official Julia version manager and multiplexer [`juliaup`](https://github.com/JuliaLang/juliaup) by running
```bash
curl -fsSL https://install.julialang.org | sh
```
This should also install the latest version of Julia.

# Best Practices

## Package and environment management

Julia ships with a package manager that can be accessed by loading the `Pkg` package:
```julia
julia> using Pkg
```
You generally do not need to load this package, unless you wish to inteface programmatically with the package manager.
The package manager can also be accessed interactively by pressing the `]` key from the Julia REPL prompt, which opens the Pkg REPL:
```julia
julia>]
(@v1.11) pkg> add BenchmarkTools
```
Notice the change of prompt indicator: `v1.11` refers to the currently active environment, which *by default* is the global environment for that current version of Julia (in this case v1.11).
You should generally *not* add packages to this environment, as they will be available to *every* project using Julia v1.11. 
You should instead restrict yourself to installing only tooling packages, such as the [BenchmarkTools.jl](https://github.com/JuliaCI/BenchmarkTools.jl) package.

For packages that go beyond simple tooling, a specific environment should *always* be used. 
Suppose we are working in the directory `MyProject`, then an environment associated with this project can be activated from Pkg prompt using
```julia
(@v1.11) pkg> activate .
(MyProject) pkg> add Plots
```
where we have then added the [Plots](https://github.com/JuliaPlots/Plots.jl) package to this environment (again notice the prompt change).
In contrast to Python's `venv`, Julia environments are very lightweight and consist of two files:

- `Project.toml`: lists all the packages used by the environment, which can be considered a list of dependencies if the environment represents a package. 
- `Manifest.toml`: is machine generated and lists the entire dependency tree down to the exact verion of the packages. This is so the exact state of environment can be reproduced.

Note, neither of these files get generated if no packages are added, i.e. simply executing `julia>]activate .` is not enough create these files if they do not exist already. 
An environment can also be activate from the command line when launching Julia:
```bash
julia --project=. myscript.jl
```
which will execute `myscript.jl` in the environment associated with the working directory.
This is where loading the package Pkg can be useful. 
One can for example first run:
```bash
julia --project=. -e "using Pkg; Pkg.instantiate()"
```
to make sure all packages appearing in the Manifest.toml are downloaded and precompiled at the correct versions.

## Development workflow and Revise.jl

Suppose you are editing a file `myadd.jl` containing the code:
```julia
# myadd.jl
function myadd(a, b)
    return a - b
end
```
A sensible workflow is to open a Julia session and include the code contained in this file into the session:
```julia
julia> include("myadd.jl")
```
The function `myadd` can now be tested:
```julia
julia> myadd(2, 3)
-1
```
It appears this function might not be doing what it intented. 
If, without exiting Julia, you make the fix:
```julia
# myadd.jl
function myadd(a, b)
    return a + b # This should now correctly add a and b.
end
```
you will notice this change is not reflected in the REPL:

```julia
julia> myadd(2, 3)
-1
```
One would need to *restart* julia and then `include` the package again, which is impracticle when testing code. 
To solve this, there is one essential package not included in the Julia standard library: [Revise.jl]("https://timholy.github.io/Revise.jl/stable/"). 
Revise.jl will automatically include update any changes to source code loaded in the REPL, without require a REPL restart, provided the file is loaded using
```julia
using Revise
julia> Revise.includet("myadd.jl") # notice the `t` in `includet`
```
Changes to locally included modules using `using Module` are also updated without a restart, provided Revise.jl is loaded first.
The Revise.jl package is a prototypical example of a package that *should* be included in your global environment. 
You could even go as far as automatically loading it everytime the Julia is launched by adding
```julia
# ~/.julia/config/startup.jl
using Revise
```
to the your `startup.jl`. The code in this file is executed whenever Julia is started, and can be disabled using:
```bash
julia --startup-file=no
```

# General Purpose GPU Programming

## CUDA

When loading the `CUDA` package, Julia will attempt to download a suitable version of the CUDA toolkit based on the devices it finds. 
The problem is that compute nodes, for example, do not have internet access, so this fails. 
Thereforw, we must tell Julia to use locally installed CUDA toolkit. 
This can be done by launching a Julia REPL and executing  
```julia
julia> using CUDA
julia> CUDA.set_runtime_version!(v"12.2"; local_toolkit=true)
```
This will create a file named `LocalPreferences.toml` in the working directory.
It is not strictly necessary to pass the version CUDA runtime version to this function, however it allows packages to precompile and may be required for some to work correctly. 
The version passed should match the CUDA runtime version installed on the node. 
The system admin should be able to tell you this information, or you can compile and run this small programme to print information about he devices available and CUDA toolkit version information.

## Other GP-GPU APIs

The Apple Silicon Metal programming framework can be interfaced with [Metal.jl](https://github.com/JuliaGPU/Metal.jl), ROCm (for AMD GPUs) with [AMDGPU.jl](https://github.com/JuliaGPU/AMDGPU.jl), and Intel's oneAPI with [oneAPI.jl](https://github.com/JuliaGPU/oneAPI.jl). 
There also exists [KernalAbstractions.jl](https://github.com/JuliaGPU/KernelAbstractions.jl?tab=readme-ov-file) which allows one to write GPU kernals *agnostic* to the backend, which may be of interest.

# MPI

The Message Passing Inferface (MPI) interface can be accessed using the [MPI.jl](https://github.com/JuliaParallel/MPI.jl) package. 
You should follow the [configuration](https://juliaparallel.org/MPI.jl/stable/configuration/) guide as, similar to accessing the CUDA interface, Julia needs to know which implementation of MPI are available on the system.
MPI.jl *will* attempt to download and install an implementation, however this will fail if Julia is executing on a HPC compute node without internet access.

# Adding private Github packages using Pkg
If you wish to add a private repo to your environment using Pkg, then the easiest way to authenticate is via the [Github CLI](https://cli.github.com). 
First install this and follow through with the configuration, and then make sure the following environment variable is set whenever you wish to `add` a private github repo:
```bash
export JULIA_PKG_USE_CLI_GIT=true
```
You should put this in you `.bashrc` (or whatever the correspoding config file is for your shell of choice)
