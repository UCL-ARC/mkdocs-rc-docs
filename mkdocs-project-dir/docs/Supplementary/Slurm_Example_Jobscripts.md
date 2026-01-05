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

## Hybrid MPI with OpenMP job

Here are two example NAMD jobs that use some MPI processes which can communicate between nodes, and some OpenMP threads inside a node.

NAMD uses charm++ which can be complex to launch.

```
#!/bin/bash
# NAMD build using verbs, has more complex launch line.

# Based on ARCHER2 mpi+smp jobscript
# 2 nodes, 2 MPI comm processes per node, 20 worker threads per comm process
#SBATCH --job-name=namd_verbs_2node
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=20
#SBATCH --nodes=2
#SBATCH --time=4:00:00

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-05
module load namd/3.0.1-verbs/gcc-12.3.0

# Set procs per node (PPN) & OMP_NUM_THREADS
# One CPU core left free for associated MPI process
export PPN=$(($SLURM_CPUS_PER_TASK-1))
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores

# Record PPN in the output file
echo "Number of worker threads PPN = $PPN"

# Cannot use srun directly with verbs version as charmrun is required 
# so Slurm options need to be passed as environment variables.
# ++verbose is being passed for more debug information on what charmrun is doing.
export SLURM_HINT=nomultithread
charmrun ++n ${SLURM_NTASKS} ++mpiexec ++remote-shell srun ++verbose $(which namd3) +setcpuaffinity ++ppn ${PPN} apoa1.namd
```

```
#!/bin/bash
# NAMD build using ucx, simpler launch line, may have more overheads.

# Based on ARCHER2 mpi+smp jobscript
# 2 nodes, 2 MPI comm processes per node, 20 worker threads per comm process
#SBATCH --job-name=namd_verbs_2node
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=20
#SBATCH --nodes=2
#SBATCH --time=4:00:00

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-05
module load namd/3.0.1-ucx/gcc-12.3.0

# Set procs per node (PPN) & OMP_NUM_THREADS
# One CPU core left free for associated MPI process
export PPN=$(($SLURM_CPUS_PER_TASK-1))
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores

# Record PPN in the output file
echo "Number of worker threads PPN = $PPN"

# Setting nomultithread can be omitted as set by default.
srun --hint=nomultithread namd3 +setcpuaffinity +ppn $PPN apoa1.namd
```

## Array job

A job array is a group of jobs with the same executable and resource requirements, but different input files. A job array can be submitted, controlled, and monitored as a single unit. Each job submitted from a job array shares the same job ID as the job array and has its own `$SLURM_ARRAY_TASK_ID`. 

At present, the maximum allowed size of a job array is 1000 tasks.

```
#!/bin/bash
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --job-name=myArray
#SBATCH --time=00:30:00
#SBATCH --array=1-10

# Each task processes a different csv data file
myprogram dataset${SLURM_ARRAY_TASK_ID}.csv
```

The array is created by Slurm directive `--array=1-10` by including elements numbered [1-10] to represent our 10 input files.

When the array job is submitted, Slurm will create 10 jobs under the single job ID. The job array script is submitted in the usual way:

```
sbatch myarrayjob
```

## Enabling Hyperthreads

Hyperthreading lets you use two virtual cores instead of one physical core - some programs can take advantage of this.

On clusters with hyperthreading enabled (Kathleen, Young, Michael), you can request it with:

```
#SBATCH --hint=multithread
```

This tells Slurm to schedule two threads per physical core.

By default we have set `--hint=nomultithread` so hyperthreads will not be used unless requested.

## Examples for Young

### CPU only jobs

#### Single node script
```
#!/bin/bash

#SBATCH --parition=cpu
#SBATCH --qos=standcpu
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --mem-per-cpu=4G
#SBATCH --time=12:00:00
#SBATCH --job-name=CPU_Single_Node

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-12
module load default-modules/2025-12

# Lines to execute below here
<application execution here>
```

#### Single node interactive session
The resource request is identical to the script above. However, once the resources are allocated you'll be migrated to the compute node and will need to then execute any lines manually.  The module commands and \<application execution here\> would be done by-hand.  You'll notice also the `--pty` flag at the end which stands for "pseudo terminal" and is letting Slurm know to start the session with `bash`.

```
srun -p cpu --qos=standcpu --nodes=1 --ntasks-per-node=8 --mem-per-cpu=4G --time=12:00:00 --pty bash -l
```

#### Multi-node script
#### Multi-node interactive session

### High-Bandwidth (HBM) memory jobs

#### HBM node script
#### HBM node interactive session

### GPU jobs

#### GPU node script
#### GPU node interactive session
