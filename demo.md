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

# without 
CC=gcc CXX=g++ uenv run prgenv-gnu/24.11:v1 --view=default -- cmake -DAFFINITY_GPU_BACKEND=cuda -DAFFINITY_MPI=on ..
```

Test it out
```
srun --uenv=prgenv-gnu/24.11:v2 --view=default -n8 -N2 ./affinity.cuda
srun --uenv=prgenv-gnu/24.11:v2 --view=default -n8 -N2 --gpus-per-task=1 ./affinity.cuda
```

## arbor

deps:
```
fmt,googletest,pugixml,nlohmann-json,random123,python,py-numpy,py-pybind11
```

https://docs.cscs.ch/build-install/uenv/
https://docs.cscs.ch/build-install/applications/wrf/#using-spack

set up spack-uenv
```
git clone https://github.com/eth-cscs/uenv-spack.git
(cd uenv-spack && ./bootstrap)
```

configure the environment
```
uenv start prgenv-gnu/24.11:v2 --view=spack,default
uenv-spack/uenv-spack $PWD/build --uarch=zen2 --specs=fmt,googletest,pugixml,nlohmann-json,random123,python,py-numpy,py-pybind11
```

build
```
cd /users/bcumming/userlab/build
./build
```

