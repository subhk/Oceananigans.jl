# Tracers

The tracers to be advected around can be specified via a list of symbols. By default the model doesn't evolve any
tracers.

```@meta
DocTestSetup = quote
    using Oceananigans
end
```

```jldoctest tracers
julia> grid = RectilinearGrid(size=(64, 64, 64), extent=(1, 1, 1));

julia> model = NonhydrostaticModel(grid=grid)
NonhydrostaticModel{CPU, Float64}(time = 0 seconds, iteration = 0)
├── grid: RectilinearGrid{Float64, Periodic, Periodic, Bounded}(Nx=64, Ny=64, Nz=64)
├── tracers: ()
├── closure: Nothing
├── buoyancy: Nothing
└── coriolis: Nothing
```

But tracers can be added with the `tracer` keyword. For example to add temperature `T` and salinity
`S`:


```jldoctest tracers
julia> grid = RectilinearGrid(size=(64, 64, 64), extent=(1, 1, 1));

julia> model = NonhydrostaticModel(grid=grid, tracers=(:T, :S))
NonhydrostaticModel{CPU, Float64}(time = 0 seconds, iteration = 0)
├── grid: RectilinearGrid{Float64, Periodic, Periodic, Bounded}(Nx=64, Ny=64, Nz=64)
├── tracers: (:T, :S)
├── closure: Nothing
├── buoyancy: Nothing
└── coriolis: Nothing
```

whose fields can be accessed via `model.tracers.T` and `model.tracers.S`.

```jldoctest tracers
julia> model.tracers.T
Field located at (Center, Center, Center)
├── data: OffsetArrays.OffsetArray{Float64, 3, Array{Float64, 3}}, size: (64, 64, 64)
├── grid: RectilinearGrid{Float64, Periodic, Periodic, Bounded}(Nx=64, Ny=64, Nz=64)
└── boundary conditions: west=Periodic, east=Periodic, south=Periodic, north=Periodic, bottom=ZeroFlux, top=ZeroFlux, immersed=ZeroFlux

julia> model.tracers.S
Field located at (Center, Center, Center)
├── data: OffsetArrays.OffsetArray{Float64, 3, Array{Float64, 3}}, size: (64, 64, 64)
├── grid: RectilinearGrid{Float64, Periodic, Periodic, Bounded}(Nx=64, Ny=64, Nz=64)
└── boundary conditions: west=Periodic, east=Periodic, south=Periodic, north=Periodic, bottom=ZeroFlux, top=ZeroFlux, immersed=ZeroFlux
```

Any number of arbitrary tracers can be appended to this list and passed to a model constructor. For example, to evolve
quantities ``C_1``, CO₂, and nitrogen as additional passive tracers you could set them up as

```jldoctest tracers
julia> model = NonhydrostaticModel(grid=grid, tracers=(:T, :S, :C₁, :CO₂, :nitrogen))
NonhydrostaticModel{CPU, Float64}(time = 0 seconds, iteration = 0)
├── grid: RectilinearGrid{Float64, Periodic, Periodic, Bounded}(Nx=64, Ny=64, Nz=64)
├── tracers: (:T, :S, :C₁, :CO₂, :nitrogen)
├── closure: Nothing
├── buoyancy: Nothing
└── coriolis: Nothing
```

!!! info "Active vs. passive tracers"
    An active tracer typically denotes a tracer quantity that affects the fluid dynamics through buoyancy. In the ocean
    temperature and salinity are active tracers. Passive tracers, on the other hand, typically do not affect the fluid
    dynamics are are _passively_ advected around by the flow field.
