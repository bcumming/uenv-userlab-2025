## affinity

Kick off a uenv session

```bash
uenv start prgenv-gnu/24.11:v1 --view=default
```

Download and install the software

```bash
git clone git@github.com:bcumming/affinity.git
mkdir affinity/build
cd affinity/build/

# go read the docs: https://github.com/bcumming/affinity
CC=gcc CXX=g++ cmake -DAFFINITY_GPU_BACKEND=cuda -DAFFINITY_MPI=on ..

# without starting a new shell
CC=gcc CXX=g++ uenv run prgenv-gnu/24.11:v1 --view=default -- cmake -DAFFINITY_GPU_BACKEND=cuda -DAFFINITY_MPI=on ..
```

Test it out
```
# fails if uenv not loaded
srun -n8 -N2 ./affinity.cuda

# load the uenv on the compute nodes
srun --uenv=prgenv-gnu/24.11:v2 --view=default -n8 -N2 ./affinity.cuda
srun --uenv=prgenv-gnu/24.11:v2 --view=default -n8 -N2 --gpus-per-task=1 ./affinity.cuda
```

## microHH

[MicroHH 2.0](https://microhh.readthedocs.io/en/latest/index.html)

Download the code:

```
git clone --recurse-submodules https://github.com/microhh/microhh.git
cd microhh
```

Let's use the `prgenv-gnu`
```
uenv start prgenv-gnu/24.11:v2 --view=default
```

Follow the docs to try to build with MPI support
```
cp config/generic.cmake config/default.cmake
mkdir build; cd build
cmake .. -DUSEMPI=TRUE
make -j12
```

We hit some compiler errors about missing headers.

It looks like the CMake tool isn't properly configured to find all dependencies

```
52c52
<         set(USER_CXX_FLAGS "-std=c++17 -I/user-environment/env/default/include")
68c68
< set(LIBS -L/user-environment/env/default/lib -L/user-environment/env/default/lib64 ${FFTW_LIB} ${FFTWF_LIB} ${NETCDF_LIB_C} ${HDF5_LIB})
```

## wrf

[docs.cscs.ch](https://docs.cscs.ch/build-install/applications/wrf/#using-spack)

