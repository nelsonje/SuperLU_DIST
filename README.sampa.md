
# Building on the Sampa cluster

```
# Use GCC 4.9.3 and Intel MPI.
module purge
module load gcc intel-mpi cmake

# Use Intel MKL.
source /sampa/share/intel/parallel_studio_xe_2016/mkl/bin/mklvars.sh intel64

# Build everything.
make
```

# Running on the Sampa cluster


SuperLU_DIST uses OpenMP for parallelism within a node by default. The number of
threads used can be controlled by the environment variable
```OMP_NUM_THREADS```. By default, all cores on a node will be used.

The real-valued solver demo code is called
```EXAMPLE/pddrive```. (there are other options in that directory
too.)

Data distribution is controlled with the ```-r``` and ```-c```
flags. The product of ```-r``` and ```-c``` should be the equal to (or
maybe less than, although I'm not sure why it would be) as number of
nodes * tasks per node. To get a 1D distribution of columns over
processes, set ```-r``` to 1 and ```-c``` to the number of processes.

Here are some example job configurations.

Run with 8 threads on a single node:
```
OMP_NUM_THREADS=8 srun --nodes=1 --ntasks-per-node=1 ./EXAMPLE/pddrive -r 1 -c 1 ~/LU2/G3_circuit.mtx
```

Run with 8 processes (not threads) on a single node (this seems to be faster):
```
OMP_NUM_THREADS=1 srun --nodes=1 --ntasks-per-node=8 ./EXAMPLE/pddrive -r 1 -c 8 ~/LU2/G3_circuit.mtx
```

Run with 8 nodes and 8 processes (not threads) per node:
```
OMP_NUM_THREADS=1 srun --nodes=8 --ntasks-per-node=8 ./EXAMPLE/pddrive -r 1 -c 8 ~/LU2/G3_circuit.mtx
```
