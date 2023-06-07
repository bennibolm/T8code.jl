# T8code.jl

[![All Tests Status](https://github.com/DLR-AMR/T8code.jl/actions/workflows/test.yml/badge.svg)](https://github.com/DLR-AMR/T8code.jl/actions/workflows/test.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-success.svg)](https://opensource.org/licenses/MIT)


[T8code.jl](https://github.com/DLR-AMR/T8code.jl) is a Julia package
that wraps [`t8code`](https://github.com/DLR-AMR/t8code), a C/C++ library to manage
multiple connected adaptive quadtrees or octrees in parallel and with possibly mixed
element types.

## Installation

If you have not yet installed Julia, please [follow the instructions for your
operating system](https://julialang.org/downloads/platform/).
[T8code.jl](https://github.com/DLR-AMR/T8code.jl) works with Julia v1.6
and later.

[T8code.jl](https://github.com/DLR-AMR/T8code.jl) is a registered Julia
package. Hence, you can install it by executing the following commands in the
Julia REPL:

```julia
julia> import Pkg; Pkg.add(["T8code", "MPI"])
```

With this command, you install both
[T8code.jl](https://github.com/DLR-AMR/T8code.jl) and
[MPI.jl](https://github.com/JuliaParallel/MPI.jl).
Currently, [T8code.jl](https://github.com/DLR-AMR/T8code.jl) supports
only builds of the C/C++ library [`t8code`](https://github.com/DLR-AMR/t8code)
with MPI support, so you need to initialize MPI appropriately as described
in the [Usage](#usage) section below.

[T8code.jl](https://github.com/DLR-AMR/T8code.jl) depends on the binary
distribution of the [`t8code`](https://github.com/DLR-AMR/t8code) library,
which is available in the Julia package T8code\_jll.jl and which is automatically
installed as a dependency. The binaries provided by T8code\_jll.jl support MPI
and are compiled against the default MPI binaries of MPI.jl. At the time of
writing, these are the binaries provided by MicrosoftMPI\_jll.jl on Windows and
MPICH\_jll.jl on all other platforms.

By default, [T8code.jl](https://github.com/DLR-AMR/T8code.jl) provides
pre-generated Julia bindings to all exported C interface functions of the underlying
[`t8code`](https://github.com/DLR-AMR/t8code) library. If you want/need to
generate new bindings, please follow the instructions in the `dev` folder and
copy the generated files to the appropriate places in `src`.

### Using a custom version of MPI and/or `t8code`

[`t8code`](https://github.com/DLR-AMR/t8code) needs to be compiled against the
same MPI implementation used by
[MPI.jl](https://github.com/JuliaParallel/MPI.jl). Thus, if you want to
configure [MPI.jl](https://github.com/JuliaParallel/MPI.jl) to not use the
default MPI binary provided by JLL wrappers, you also need to build
[`t8code`](https://github.com/DLR-AMR/t8code) locally using the same MPI
implementation. This is typically the situation on HPC clusters. If you are
just using a single workstation, the default installation instructions should
be sufficient.

[T8code.jl](https://github.com/DLR-AMR/T8code.jl) allows using a
[`t8code`](https://github.com/DLR-AMR/t8code) binary different from the default
one provided by T8code\_jll.jl.
To enable this, you first need to obtain a local binary installation of
[`t8code`](https://github.com/DLR-AMR/t8code). Next, you need to configure
[MPI.jl](https://github.com/JuliaParallel/MPI.jl) to use the same MPI
implementation used to build your local installation of
[`t8code`](https://github.com/DLR-AMR/t8code), see
[the documentation of MPI.jl](https://juliaparallel.org/MPI.jl/stable/configuration/).
At the time of writing, this can be done via

```julia
julia> using MPIPreferences

julia> MPIPreferences.use_system_binary()
```

if you use the default system MPI binary installation to build
[`t8code`](https://github.com/DLR-AMR/t8code).

Next, you need to set up the
[Preferences.jl](https://github.com/JuliaPackaging/Preferences.jl)
setting containing the path to your local build of the shared library of
[`t8code`](https://github.com/DLR-AMR/t8code).

```julia
julia> using Preferences, UUIDs

julia> set_preferences!(
           UUID("d0cc0030-9a40-4274-8435-baadcfd54fa1"), # UUID of T8code.jl
           "libt8" => "/path/to/your/libt8.so", force = true)
```

One has to repeat the same procedure for [P4est.jl](https://github.com/trixi-framework/P4est.jl)
as described in [Using a custom version of MPI and/or p4est](https://github.com/trixi-framework/P4est.jl#using-a-custom-version-of-mpi-andor-p4est).

Note that you should restart your Julia session after changing the preferences.

Currently, custom builds of [`t8code`](https://github.com/DLR-AMR/t8code)
without MPI support are not supported.

## Issues

MPI parallelization segfaults.

## Authors

T8code.jl is mainly maintained by [Johannes Markert](https://jmark.de) (German
Aerospace Center (DLR), Germany).  It is an adapted fork from
[P4est.jl](https://github.com/trixi-framework/P4est.jl) maintained by [Michael
Schlottke-Lakemper](https://lakemper.eu) (RWTH Aachen University, Germany) and
[Hendrik Ranocha](https://ranocha.de) (University of Hamburg, Germany).

The [`t8code`](https://github.com/DLR-AMR/t8code) library itself is written by
Johannes Holke and Carsten Burstedde, and others.

## License and contributing

T8code.jl is licensed under the MIT license (see [LICENSE.md](LICENSE.md)).
[`t8code`](https://github.com/DLR-AMR/t8code) itself is licensed under the GNU
General Public License, version 2.
