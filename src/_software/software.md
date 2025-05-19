---
title: "TimeEvolutionPEPO.jl"
---

This is a Julia package. Julia can be installed by running, in your terminal:
```bash
curl -fsSL https://install.julialang.org | sh
```
which installs both `julia` and the Julia version manager `juliaup`.

# Installation

To use this package, first create a new directory (or navigate to an existing directory) and start Julia from the command line:
```bash
mkdir MyProject
cd MyProject
julia
```
You should then activate an enviroment withing the directory to avoid polluting your global enviroment.
First open the package manager interface by typing `]` at the Julia prompt:
```julia
julia>]
```
Then from the package interface, 
```julia
(@v1.11) pkg> activate .
```
which activates (or creates) an enviroment in current directory, indiciated by the `.`.
Packages can then be added to this enviroment:
```julia
(MyProject) pkg> add TimeEvolutionPEPO
```
which will install the TimeEvolutionPEPO package, and add it to the enviroment. 
Note the change in prompt from `(@v1.11)` (the global enviroment) to `(MyProject)` (the local enviroment).
Additional packages that might be useful can also be added:
```julia
(MyProject) pkg> add Plots, DataFrames
```

# Usage

```julia
function thermalising(; D = 2)

    # Initialise to the infinite temperate thermal state on 2x2 unit cell lattice
    rho = fill(ThermalState(),2,2)

    # Define the square lattice Ising model critical temperature
    βc = log(1 + sqrt(2)) / 2

    # Construct the model itself
    H = Heisenberg(;  Jz = -1.0);

    # Define a method, here we use time-evolving block decimation (TEBD)
    method = TEBD(
        numsteps=1000, 
        timestep=0.002*βc, 
        truncalg=PEPOKit.SU(ξ = xi)
    )

    # We define the interactions to be the same on each axis.
    model = [OnAxis(H,1), OnAxis(H,2)]

    # We also need to define how we compute observables. Here we use the VUMPS algorithm
    obsalg = VUMPS(bonddim=16, maxiter=200, tol=1e-10)

    # The local Hilbert space is set to a non-symmetric vector space of dimension 2
    localspace = ComplexSpace(2)

    # The bond dimension can be controlled
    bondspace = ComplexSpace(D)

    # We then set up the problem we wish to solve
    sim = Simulation(model, rho, bondspace; physical = localspace, method = method)

    # Define an empty vector to store our output
    xis = Float64[]

    # Run the simulation!
    simulate!(sim; maxshots=100) do simstep

        # Compute the density matrix
        dm = DensityMatrix(quantumstate(simstep), obsalg)

        # Append the correlation length to the output vector
        push!(xis, correlationlength(dm))
    end

    return xis
end
```


