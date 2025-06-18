# Slurm Example Jobscripts

## Multi-node MPI job

In this example all modules are purged and then reloaded. This is not necessary, but since your current environment gets copied to the job, this makes sure that the batch job will still behave in the same way even if you are working with different local modules interactively on the login node you are submitting from. 

```
#!/bin/bash

#SBATCH --job-name=vasp_6.4.3_2node
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=40
#SBATCH --cpus-per-task=1
#SBATCH --time=4:00:00

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-05
module load mpi/intel-oneapi-mpi/2021.14.0/intel-oneapi-2024.2.1
module load intel-oneapi-mkl/2023.2.0-intel-oneapi-mpi/intel-oneapi-2024.2.1
module load hdf5/1.14.3-intel-oneapi-mpi/oneapi-2024.2.1
module load vasp/6.4.3-intel-oneapi-mpi/oneapi-2024.2.1

srun vasp_std
```

## Enabling Hyperthreads

Hyperthreading lets you use two virtual cores instead of one physical core - some programs can take advantage of this.

On clusters with hyperthreading enabled (Kathleen, Young, Michael), you can request it with:

```
#SBATCH --hint=multithread
```

This tells Slurm to schedule two threads per physical core.

By default we have set `--hint=nomultithread` so hyperthreads will not be used unless requested.

